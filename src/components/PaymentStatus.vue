<template>
  <div>
    <div class="card">
      <div class="card-body">
        <div class="row d-flex align-items-start">
          <div class="col-lg-6 no-gutters">
            <figure>
              <img alt="Baju Futsal CDC Helang" class="img-fluid" src="../assets/baju-futsal.jpeg" />
            </figure>
          </div>
          <div class="col-lg-6">
            <figure>
              <a rel="noopener noreferrer" href="https://toyyibpay.com/e/143251051126981" target="_blank">
              <img src="../assets/toyyibpay.png" class="img-fluid" alt="onewoorks-toyyibpay" />
              </a>
            </figure>

            <div v-if="payment_status != null">  
                <div v-if="payment_status.billpaymentStatus == 3" class="alert alert-danger">
                    Payment not success, Please try again
                </div>

                <div v-if="payment_status.billpaymentStatus == 1" class="alert alert-success">
                    Payment Success, Thank you
                </div>

                <div v-if="payment_status.billpaymentStatus == 2" class="alert alert-warning">
                    Pending payment, please check again.
                </div>

              <transition name="slide-fade">
                <div>
                  <table class="table table-bordered table-sm">
                    <tbody>
                      <tr>
                        <th scope="col">Transaction Code</th>
                        <td class="text-center">{{ payment_status.billpaymentInvoiceNo}}</td>
                      </tr>
                      <tr>
                        <th scope="col">Payment Status</th>
                        <td>{{ payment_status_label.toUpperCase() }}</td>
                      </tr>
                      <tr>
                        <th scope="col">Bill Code</th>
                        <td>{{ payment_status.billPermalink }}</td>
                      </tr>
                      <tr>
                        <th scope="col">Order Id</th>
                        <td>{{ $route.query.order_id }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </transition>

              <div @click="go_to_payment" v-if="payment_status.billpaymentStatus == 3" class='btn btn-block btn-primary'>
                  Try Again
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import { stat } from 'fs';
export default {
  data: function() {
    return {
      payment_status: null,
      payment_status_label: null
    };
  },
  mounted: function() {
      Axios.post('https://api.onewoorks-solutions.com/payment_gateway/toyyibpay/transaction', this.$route.query )
      .then(response => {
          let actual = response.data
          this.payment_status = actual;
          let status_name = ""
          switch(actual.billpaymentStatus){
              case "1":
                  status_name = "Payment Completed"
                  break;
            case "2":
                status_name = "Pending Payment"
                break;
                case "3":
                    status_name = "Payment Failed"
                    break;
          }
          this.payment_status_label = status_name
      })    
  },
  methods: {
      go_to_payment: function(){
        window.location.href = `https://toyyibpay.com/${this.payment_status.billcode}`    
      }
      
  }
};
</script>

<style scoped>
figure {
  margin: 0;
}
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>