<template>
  <div>
    <div class="card">
      <div class="card-body">
        <div class="row align-items-center h-100">
          <div class="col-lg-6 no-gutters">
            <figure>
              <img alt="Baju Futsal CDC Helang" class="img-fluid" src="../assets/baju-futsal.jpeg" />
            </figure>
          </div>
          <div class="col-lg-6 mx-auto contentnya">
            <form @submit.prevent="how_much">
              <br>
              <input
                v-if="input_no != false"
                type="text"
                v-model="pick_no"
                class="form-control text-center"
                placeholder="Sila masukkan no baju anda!"
              />
              <div>
                <br />
                <transition name="slide-fade">
                <div v-if="detail != null">
                  <table class="table table-bordered table-condensed">
                    <tbody>
                      <tr>
                        <th>Nama</th>
                        <td>{{ detail.nama.toUpperCase() }}</td>
                      </tr>
                      <tr>
                        <th>NickName</th>
                        <td>{{ detail.nickname }}</td>
                      </tr>
                      <tr>
                        <th>No</th>
                        <td>{{ detail.no}}</td>
                      </tr>
                      <tr>
                        <th>Saiz Baju</th>
                        <td>{{ detail.size }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>

                <div v-if="duplicates != null">

                  <div class="alert alert-info text-left">Pick your name!</div>
                  <ul v-for="(duplicate, index) in duplicates" :key="index" class=''>
                    <li class='text-left' @click="pick_me(duplicate)">{{ duplicate.nama.toUpperCase() }}</li>
                  </ul>
                </div>
                </transition>
 

                <div class="btn-group btn-block" v-if="btn_check != null">
                  <div @click="search_again" class='btn btn-outline-primary'>Search Again</div>
                  <button
                type="button"
                  v-if="btn_check == 'payment'"
                  class="btn btn-success"
                  @click="go_to_payment"
                >Proceed To Payment</button>
                </div>
                
                <ToyyibPay :person="detail" v-if="payment_confirm == true" />
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import order_list_data from "@/data/order-list.json";
import ToyyibPay from '@/components/payment_gateway/ToyyibPay'
export default {
  components:{
    ToyyibPay
  },
  data: function() {
    return {
      input_no: true,
      pick_no: null,
      detail: null,
      btn_check: null,
      payment_uri: null,
      duplicates: null,
      payment_confirm: false
    };
  },
  methods: {
    more_than_one: function(payloads){
      this.duplicates = payloads
    },
    view_picker_info: function(info){
        this.detail = info;
        this.url_payment(this.detail.size);
        this.btn_check = "payment";
    },
    how_much: function() {
      this.detail = null;
      this.payment_confirm = false
      let arr = order_list_data;
      let jumpa = []
      for(let i = 0; i<arr.length; i++){
        if(arr[i].no == parseInt(this.pick_no)){
          jumpa.push(arr[i])
        }
      }
      if(jumpa.length>1){
        this.more_than_one(jumpa)
      } else {
        this.view_picker_info(jumpa[0])
      }
    },
    url_payment: function(size_baju) {
      let uri_path = "";
      switch (size_baju) {
        case "S":
          uri_path = "https://www.billplz.com/4eqfphi_2";
          break;
        case "M":
          uri_path = "https://www.billplz.com/c_qvqtkc7";
          break;
        case "L":
          uri_path = "https://www.billplz.com/hu77gxl30";
          break;
        case "XL":
          uri_path = "https://www.billplz.com/gdkxhmvr2";
          break;
        case "XXL":
          uri_path = "https://www.billplz.com/gb4yz5ml0";
          break;
      }
      this.payment_uri = uri_path;
    },
    go_to_payment: function() {
      // window.location.href = this.payment_uri;
      this.btn_check = null
      this.payment_confirm = true
      this.input_no = false
    },
    search_again: function(){
      this.detail = null
      this.pick_no = null
      this.payment_uri = null
      this.btn_check = null
    },
    pick_me: function(me){
      this.duplicates = null
      this.view_picker_info(me)
    }
  }
};
</script>

<style scoped>
figure {
  margin: 0
}
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>