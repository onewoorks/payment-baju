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
            <figure>
              <img src='../assets/toyyibpay.png' class='img-fluid' alt="onewoorks-toyyibpay" v-if="payment_confirm == true"  />
            </figure>
            
            <form @submit.prevent="how_much">
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
                  <table class="table table-bordered table-sm">
                    <tbody>
                      <tr>
                        <th scope="col">Nama</th>
                        <td>{{ detail.nama.toUpperCase() }}</td>
                      </tr>
                      <tr>
                        <th scope="col">NickName</th>
                        <td>{{ detail.nickname }}</td>
                      </tr>
                      <tr>
                        <th scope="col">No</th>
                        <td>{{ detail.no}}</td>
                      </tr>
                      <tr>
                        <th scope="col">Saiz Baju</th>
                        <td>{{ detail.size }}</td>
                      </tr>
                      <tr v-if="payment_detail != null">
                        <th scope="col">Payment Status</th>
                        <td>{{ payment_detail.toUpperCase() }}</td>
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

                <div v-if="btn_check == null && payment_confirm == false " @click="how_much" class="btn btn-block btn-primary">Search</div>
                <div class="btn-group btn-block" v-if="btn_check != null">
                  <div @click="search_again" class='btn btn-outline-primary'>Search Again</div>
                  <button
                type="button"
                  v-if="btn_check == 'payment'"
                  class="btn btn-success"
                  @click="go_to_payment"
                >Proceed To Payment</button>
                <button
                type="button"
                  v-if="btn_check == 'toyyibpay'"
                  class="btn btn-success"
                  @click="go_to_toyyibpay"
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

import Axios from 'axios'
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
      payment_confirm: false,
      payment_detail: null,
      toyyib_code: null
    };
  },
  watch: {
    pick_no: function(){
      this.detail = null
      this.btn_check = null
      this.payment_uri = null
      this.duplicates = null
      this.payment_confirm = false
      this.payment_detail = null
      this.toyyib_code = null
    }
  },
  methods: {
    go_to_toyyibpay: function(){
        window.location.href = `https://toyyibpay.com/${this.toyyib_code}`
    },
    check_ref: function(response){
      if (response.length > 0){
          this.payment_detail = response[0].payment_status
          this.toyyib_code = response[0].payment_ref
          if (response[0].status == 1){
            this.btn_check = "completed"
            this.payment_confirm = false
          } else {
            this.btn_check = 'toyyibpay'
          }
          
      } else {
        this.btn_check = "payment";
        this.payment_detail = null
      }
    },
    more_than_one: function(payloads){
      this.duplicates = payloads
    },
    view_picker_info: function(info){
      if (typeof info !== "undefined"){
this.detail = info;
        
        Axios.get(`https://api.onewoorks-solutions.com/payment_gateway/toyyibpay/find?refid=${this.detail.ref_key}`)
        .then(response => {
          this.check_ref(response.data)
        })
      }
        
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