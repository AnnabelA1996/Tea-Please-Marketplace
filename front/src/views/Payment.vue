<template>
  <!-- Payment + Delivery + Error Validation - Keely -->
  <div class="app-wrapper">
    <BackButton />
    <div
      v-if="user"
      class="payment"
    >
      <!-- <h2>Enter Your Details</h2> -->
      <form
        id="payment-form"
        @submit.prevent="checkForm"
      >
        <div class="container1">
          <h2>Enter Your Details</h2>
          <div class="form-group">
            <label for="input-cardname">Name On Card</label>
            <input
              v-model="card.name"
              type="text"
              name="input-cardname"
              placeholder="e.g: John Doe"
              required
            >
          </div>
          <span id="card-name-error"><p>Please Enter A Valid Name</p></span>
          <div class="form-group">
            <label for="input-cardnumber">Card Number</label>
            <input
              v-model="card.number"
              type="text"
              name="input-cardnumber"
              placeholder="4111-2222-3333-4444"
              required
            >
          </div>
          <span id="number-error"><p>Please Enter A Valid Card Number</p></span>
          <div class="exp-cvc-inputs">
            <div
              class="form-group expiry-group"
            >
              <label for="input-expdate">Expiry Date</label>
              <input
                v-model="card.expiry"
                type="text"
                name="input-expdate"
                placeholder="MM / YY"
                maxlength="7"
                required
              >
            </div>
            <span id="expiry-error"><p>Please Enter A Valid Expiry Date</p></span>
            <div
              id="smallerInputs"
              class="form-group"
            >
              <label for="input-cvc">CVC</label>
              <input
                id="input-cvc"
                v-model="card.cvc"
                type="text"
                name="input-cvc"
                maxlength="4"
                required
              >
            </div>
            <span id="cvc-error"><p>Please Enter A Valid CVC</p></span>
          </div>
        </div>
        <hr>
        <div class="container2">
          <h2>Delivery Details</h2>
          <div class="form-group">
            <label for="input-cardname">First Name</label>
            <input
              v-model="delivery.firstName"
              type="text"
              name="input-cardname"
              placeholder="e.g: John"
              required
            >
          </div>
          <span id="first-name-error"><p>Please Enter A Valid Name</p></span>
          <div class="form-group">
            <label for="input-surname">Surname</label>
            <input
              v-model="delivery.surname"
              type="text"
              name="input-surname"
              placeholder="eg: Doe"
              required
            >
          </div>
          <span id="surname-error"><p>Please Enter A Valid Name</p></span>
          <div class="form-group">
            <label for="input-email">Email Address</label>
            <input
              v-model="delivery.email"
              type="email"
              name="input-email"
              placeholder="eg: example@eg.com"
              required
            >
          </div>
          <span id="email-error"><p>Please Enter A Valid Email</p></span>
          <div class="form-group">
            <label for="input-cardnumber">Street Address</label>
            <input
              v-model="delivery.address"
              type="text"
              name="input-cardnumber"
              placeholder="eg: 12 example st, region, postcode"
              required
            >
          </div>
          <span id="address-error"><p>Please Enter A Valid Address</p></span>
          <div class="form-group">
            <label for="input-cardnumber">Delivery Instructions</label>
            <textarea
              id="input-cardnumber"
              v-model="delivery.message"
              type="text"
              name="input-cardnumber"
              placeholder="Type your message here ..."
            />
          </div>
          <div id="cancel">
            <button type="submit">
              Finalise Purchase
            </button>
            <a @click="$router.push(`/listings/`);">Cancel Purchase</a>
          </div>
        </div>
      </form>
    </div>
    <UserErrorMessage v-else />
  </div>
</template>

<script>
import UserErrorMessage from '../components/UserErrorMessage.vue';
import BackButton from '../components/BackButton.vue';

export default {
  components: {
    UserErrorMessage,
    BackButton,
  },
  props: {
    listingId: String,
    user: Object,
  },
  data() {
    return {
      isError: false,
      card: {
        number: null,
        name: null,
        cvc: null,
        expiry: null,
      },
      delivery: {
        firstName: null,
        surname: null,
        email: null,
        address: null,
      },
    };
  },
  methods: {
    checkForm() {
      this.isError = false;
      if (!this.card.name) {
        document.getElementById('card-name-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('card-name-error').style.display = 'none';
      }
      if (!this.card.number) {
        document.getElementById('number-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('number-error').style.display = 'none';
      }
      if (!this.card.expiry) {
        document.getElementById('expiry-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('expiry-error').style.display = 'none';
      }
      if (!this.card.cvc) {
        document.getElementById('cvc-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('cvc-error').style.display = 'none';
      }
      if (!this.delivery.firstName) {
        document.getElementById('first-name-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('first-name-error').style.display = 'none';
      }
      if (!this.delivery.surname) {
        document.getElementById('surname-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('surname-error').style.display = 'none';
      }
      if (!this.delivery.email) {
        document.getElementById('email-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('email-error').style.display = 'none';
      }
      if (!this.delivery.address) {
        document.getElementById('address-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('address-error').style.display = 'none';
      }
      const cardNumberRegex = /^4[0-9]{12}(?:[0-9]{3})?$/;
      if (!cardNumberRegex.test(this.card.number)) {
        document.getElementById('number-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('number-error').style.display = 'none';
      }
      const cardExpiryRegex = /^(0[1-9]|1[0-2])\/?([0-9]{4}|[0-9]{2})$/;
      if (!cardExpiryRegex.test(this.card.expiry)) {
        document.getElementById('expiry-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('expiry-error').style.display = 'none';
      }
      const cardCvc = /^[0-9]{3,4}$/;
      if (!cardCvc.test(this.card.cvc)) {
        document.getElementById('cvc-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('cvc-error').style.display = 'none';
      }
      const addressRegex = /^\s*\S+(?:\s+\S+){2}/;
      if (!addressRegex.test(this.delivery.address)) {
        document.getElementById('address-error').style.display = 'block';
        this.isError = true;
      } else {
        document.getElementById('address-error').style.display = 'none';
      }
      console.log(this.isError);
      if (this.isError === false) {
        this.completePayment();
      }
    },
    async completePayment() {
      const response = await fetch(
        `http://localhost:3000/listings/${this.listingId}/sold`,
        {
          method: 'PATCH',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            isAvaliable: 'false',
          }),
        },
      );
      const data = await response.json();
      console.log(data);
      this.$router.push(`/payment-confirmation/${this.listingId}`);
    },
  },
};
</script>

<style scoped>
.app-wrapper{
  background-color: white;
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 126vh;
}
.payment{
  margin-top: 5.5em;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
form {
  width: 80vw;
  height: 35vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  justify-content: space-between;
}

input,
textarea {
  width: 100%;
}

input,
textarea,
select {
  border: thin #a26360 solid;
  border-radius: 3px;
  padding: 0.6em 0.4em;
}

select {
  background-color: white;
  padding: 0.5em 0.4em;
}

textarea {
  height: 10vh;
  font-family: 'Questrial', sans-serif;
  resize: none;
}

input:focus, textarea:focus {
  outline: none;
}

.form-group {
  width: 90%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

label {
  margin-bottom: 0.3em;
  margin-top: 1em;
}

button {
  background-color: #a9c596;
  color: #2b463c;
  padding: 0.8em 1.5em;
  border-radius: 5px;
  border: none;
  margin-top: 2em;
}
form h2{
  margin-bottom: 0.5em;
}
.exp-cvc-inputs{
  display: flex;
  width: 80vw;
  margin-left: 2em;
}

.exp-cvc-inputs input{
  width: 82%;
  padding: 0.56em 0.1em;
}

.exp-cvc-inputs:focus{
  outline: none;
}
a{
  text-align: center;
  font-size: 0.9em;
  margin: 1em;
  padding-bottom: 1.5em;
}
hr{
  width: 75vw;
  border-top: thin #a26360 solid;
  margin: 1em 0;
  margin-top: 2em;
}
#card-name-error, #number-error, #expiry-error, #cvc-error {
  display: none;
  color: red;
}
#first-name-error,#surname-error,#email-error,#address-error{
  display: none;
  color: red;
}
#cancel{
  width: 80vw;
  display: flex;
  flex-direction: column;
  align-items: center;
}
@media screen and (min-width: 768px) {
  .payment{
    font-size: 1.2em;
    margin-top: 5em;
  }
  label{
    margin-top: 0.5em;
  }
  input{
    height: 3vh;
  }
  button{
    padding-bottom: 1.8em;
    font-size: 0.8em;
    margin: 1.5em auto;
  }
}
@media screen and (min-width: 1024px) {
  #payment-form{
    width: 100vw;
    display: flex;
    flex-direction: row;
    margin-top: 5em;
  }
  hr{
  display: none;
  }
  .container1{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 50vw;
    height: 60vh;
    margin-top: 5em;
  }
  .container2{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 14.5em;
    width: 50vw;
  }
  .exp-cvc-inputs{
    width: 60%;
    margin-left: 3.5em;
  }
  .expiry-group{
    width: 5vw;
    padding-right: 1em;
  }
  #smallerInputs{
    padding-right: 1em;
  }
  .form-group{
    width: 50%;
  }
  h2{
    margin-bottom: 2em;
  }
}
</style>
