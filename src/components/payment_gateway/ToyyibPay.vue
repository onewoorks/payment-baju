<template>
  <div>
    <form>
        {{ errors}}
      <div>
        <input
          type="text"
          name
          class="form-control text-center"
          v-model="person.nama"
          placeholder="Nama"
        />
      </div>

      <div style="margin-top:10px;">
        <input
          type="email"
          class="form-control text-center"
          placeholder="Your email address, for payment validation purposes"
          v-model="email"
        />
      </div>
      <div style="margin-top:10px;">
        <input
          type="text"
          class="form-control text-center"
          placeholder="Phone No"
          v-model="phone_no"
        />
      </div>
      <br />
      <div @click="checkForm" class="btn btn-success btn-block">Make Payment</div>
    </form>
  </div>
</template>

<script>
import Axios from 'axios'

export default {
    props: ["person"],
    data: function(){
        return {
            payer: this.person,
            email: null,
            phone_no: null,
            errors: null,
            payment_url: null
        }
    },
    methods: {
        checkForm: function (e) {
        if (this.person.nama && this.email && this.phone_no) {
            this.errors = null
        this.create_bill()
      }

      this.errors = [];

      if (!this.person.nama) {
        this.errors.push('Name is required.');
      }
      if (!this.email) {
        this.errors.push('Email is required.');
      }
      if (!this.phone_no) {
        this.errors.push('Phone no is required.');
      }

      e.preventDefault();
    },
        create_bill: function(){
            this.person['email'] = this.email  
            this.person['phone_no'] = this.phone_no          
            let buyer_payloads = {
                userSecretKey           : "ejhh3dwb-ty9q-3h6s-apt8-cn6xmcad0df7",
                categoryCode            : "3i32cume",
                billName                : "JERSEY FUTSAL CDC HELANG",
                billDescription         : "JERSEY FUTSAL CDC HELANG",
                billPriceSetting        : 1,
                billPayorInfo           : 1,
                billAmount              : 3700,
                billReturnUrl           : "https://cdc.onewoorks-solutions.com",
                billCallbackUrl         : 'https://cdc.onewoorks-solutions.com/paystatus',
                billExternalReferenceNo : this.person.ref_no,
                billTo                  : this.person.nama,
                billEmail               : this.person.email,
                billPhone               : this.person.phone_no,
                billSplitPayment        : 0,
                billSplitPaymentArgs    : "",
                billPaymentChannel      : 0
            }
            Axios.post('https://onewoorks-solutions.com/api/payment_gateway/toyyibpay.php', JSON.stringify(buyer_payloads))
            .then(response => {
                window.location.href="https://toyyibpay.com/" + response.data
            })
        }
    }
}
</script>