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
                type="text"
                v-model="pick_no"
                class="form-control text-center"
                placeholder="Sila masukkan no baju anda!"
              />
              <div>
                <br />
                <transition name="slide-fade">
                <div v-if="detail != null">
                  <table class="table table-bordered">
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
                </transition>
                <br />
                <button
                  v-if="btn_check == null"
                  class="btn btn-primary btn-block"
                  @click="how_much"
                >Check</button>

                <div class="btn-group btn-block" v-if="btn_check != null">
                  <div @click="search_again" class='btn btn-outline-primary'>Search Again</div>
                  <button
                type="button"
                  v-if="btn_check == 'payment'"
                  class="btn btn-success"
                  @click="go_to_payment"
                >Proceed To Payment</button>
                </div>
                
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
export default {
  data: function() {
    return {
      pick_no: null,
      detail: null,
      btn_check: null,
      payment_uri: null
    };
  },
  methods: {
    how_much: function() {
      this.detail = null;
      let arr = order_list_data;
      let obj = arr.find(o => o.no === parseInt(this.pick_no));
      if (obj != 0) {
        this.detail = obj;
        this.url_payment(this.detail.size);
        this.btn_check = "payment";
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
      window.location.href = this.payment_uri;
    },
    search_again: function(){
      this.detail = null
      this.pick_no = null
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