<template>
  <div class="container">
    <div class="card">
      <div class="card-body">
        <figure>
          <img alt="Baju Futsal CDC Helang" class="img-fluid" src="../assets/baju-futsal.jpeg" />
        </figure>
        <input
          type="text"
          v-model="pick_no"
          class="form-control text-center"
          placeholder="Sila masukkan no baju anda!"
        />
        <div>
          <br />
          <div v-if="detail != null">
            <table class="table table-bordered">
              <tbody>
                <tr>
                  <th>Nama</th>
                  <td>{{ detail.nama}}</td>
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
          <br />
          <button v-if="btn_check == null" class="btn btn-primary btn-block" @click="how_much">Check</button>
          <button
            v-if="btn_check == 'payment'"
            class="btn btn-success btn-block"
            @click="go_to_payment"
          >Proceed To Payment</button>
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
      btn_check: null
    };
  },
  methods: {
    how_much: function() {
        this.detail = null
      let arr = order_list_data;
      let obj = arr.find(o => o.no === parseInt(this.pick_no));
        if (obj != 0){
            this.detail = obj;
      this.btn_check = "payment";
        }
      
    },
    go_to_payment: function() {
      window.location.href = "https://www.billplz.com/4eqfphi_2";
    }
  }
};
</script>