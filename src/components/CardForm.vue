<template>
  <form class="card_form" autocomplete="off" id="add_card">
   
    <label for="card_number">
      Card number
      <span v-if="!validate.cardNumber"
        class="notValid"
      >{{messages.cardNumberMessage}}</span>
    </label>
    <input
      name="card_number"
      class="card_holder"
      v-model="cardNumberValue"
      @input="changedCardNumber"
      id="card_number"
      maxlength="16"
      type="text"
      placeholder="XXXX XXXX XXXX XXXX"
    />
    <label for="card_holder">
      Cardholder name
      <span v-if="!validate.cardHolder"
        class="notValid"
      >{{messages.cardHolderMessage}}</span>
    </label>
    <input
      name="card_holder"
      v-model="cardHolderValue"
      type="text"
      id="card_holder"
      placeholder="FIRSTNAME LASTNAME"
      @input="changedCardHolder"
    />

    <label for="valid_date">
      Valid thru
      <span v-if="!validate.validThru"
      class="notValid"
      >{{messages.validThruMessage}}</span>
    </label>
    <input
      name="valid_thru"
      maxlength="5"
      v-model="validThruValue"
      type="text"
      id="valid_date"
      @input="changedValidThru"
      placeholder="MM/YY"
    />
    <label for="cvv_code">
      CVV
       <a href="#"  @click="showCvv = !showCvv">
         <link rel="stylesheet" 
        href="https://use.fontawesome.com/releases/v5.2.0/css/all.css" 
        integrity="sha384-hWVjflwFxL6sNzntih27bfxkr27PmbbK/iSvJ+a4+0owXq79v+lsFkW54bOGbiDQ" 
        crossorigin="anonymous">
         <span v-if="showCvv">
           <i class="fa fa-eye-slash"></i>
          </span>

           <span v-if="!showCvv">
            <i class="fa fa-eye"></i>
          </span>
          
       </a>
      <span v-if="!validate.cvvCode"
      class="notValid">{{messages.cvvMessage}}</span>
    </label>
    <input
      maxlength="3"
      v-model="cvvValue"
      :type="!showCvv ? 'password' : 'text'"
      id="cvv_code"
      @input="changedCvvValue"
    />
    <select @change="validateVendor" v-model="card.selected" name="vendors" id="vendor_select">
      <option value>Please select</option>
      <option v-for="item in vendorOption" v-bind:key="item.key" v-bind:value="item.value">
          {{ item.key }}
  </option>
    </select>
    <div class="buttons">
      <button class="button" type="button" @click.prevent="updateCards">Add</button>
      <button class="button" type="button" @click="cancel">Clear</button>
      <button class="button" type="button" @click="$router.push('/')">Back</button>
</div>
  </form>
</template>

<script>
export default {
  name: "cardform",
  components: {},
  props: {
    cardNumberValue: String,
    cardHolderValue: String,
    validThruValue: String,
    cvvValue: String
  },
  data: () => {
    return {
      showCvv: false,
      card: {
        cardNumber: "",
        cardHolder: "",
        validThru: "",
        cvvCode: "",
        selected: ""
      },
      vendorOption: [
            { key: 'Bitcoin' , value: 'vendor-bitcoin'},
            { key: 'Ninja' , value: 'vendor-ninja'},
            { key: 'Block Chain' , value: 'vendor-blockchain'},
            { key: 'Evil Corp' , value: 'vendor-evil'},
          ],
      validate: {
        cardNumber: false,
        cardHolder: false,
        validThru: false,
        cvvCode: false,
        selected: false
      },
      messages: {
        cardNumberMessage: "",
        cardHolderMessage: "",
        validThruMessage: "",
        cvvMessage: "",
        responseMessage: ""
      }
    };
  },
  watch: {
    selected(value) {
      this.$emit("cardLogo", value);
    }
  },
  methods: {
    validateVendor() {
      if (this.selected == "") {
        this.validate.selected = false;
      } else {
        this.validate.selected = true;
      }
    },
    validateCvv() {
     let cvvValidate = /^[1-9]{3}$/;
      let cvvCode = document.getElementById("cvv_code").value;

      if (cvvValidate.test(cvvCode)) {
        this.validate.cvvCode = true;
        this.messages.cvvMessage = "";
      } else {
        this.validate.cvvCode = false;
        return (this.messages.cvvMessage = "cvv should only be numbers");
      }
    },
    validateValidThru() {
      let validDate = document.getElementById("valid_date").value;
      let splitDate = validDate.split("/");

      if (splitDate.length<2) {
        this.validate.validThru = false;
        return (this.messages.validThruMessage = "Please enter in MM/YY format");
      } else if(splitDate[0] <0 || splitDate[0] > 12) {
        this.validate.validThru = false;
        return (this.messages.validThruMessage = "Please enter correct month");
      } else {
         this.validate.validThru = true;
        this.messages.validThruMessage = "";
       
      }
    },
    validateCardNumber() {
      let cardNumberValidate = /\d{16}/;
      let cardNumber = document.getElementById("card_number").value;

      if (cardNumberValidate.test(cardNumber)) {
        this.validate.cardNumber = true;
        this.messages.cardNumberMessage = "";
      } else {
        this.validate.cardNumber = false;
        return (this.messages.cardNumberMessage =
          "Card number should be 16 digits");
      }
    },
    validateCardHolder() {
      let cardHolderValidate = /^[A-Za-z ]+$/;
      let cardHolder = document.getElementById("card_holder").value;

      if (cardHolderValidate.test(cardHolder)) {
        this.validate.cardHolder = true;
        this.messages.cardHolderMessage = "";
      } else {
        this.validate.cardHolder = false;
        return (this.messages.cardHolderMessage = "Name cannot be numbers");
      }
    },
    changedCardNumber() {
      this.card.cardNumber = event.target.value;
      this.$emit("changeCardNumber", this.card.cardNumber);
    },
    changedCardHolder() {
      this.card.cardHolder = event.target.value;
      this.$emit("changeCardHolder", this.card.cardHolder);
    },
    changedValidThru() {
      this.card.validThru = event.target.value;
      this.$emit("changeValidThru", this.card.validThru);
    },
    changedCvvValue() {
      this.card.cvvCode = event.target.value;
      this.$emit("changeCvvValue", this.card.cvvCode);
    },
    switchVisibility() {
      const cvvField = document.getElementById("cvv_code");
      if (cvvField.getAttribute('type') === 'password') cvvField.setAttribute('type', 'text')
      else cvvField.setAttribute('type', 'password')
    },
    updateCards() {
      this.validateVendor();
      this.validateCardNumber();
      this.validateCardHolder();
      this.validateValidThru();
      this.validateCvv();
      this.validateVendor();

      if (
        this.validate.cardNumber == false ||
        this.validate.cardHolder == false ||
        this.validate.validThru == false ||
        this.validate.cvvCode == false ||
        this.validate.selected == false
      ) {
        return;
      } else {
        this.$emit("updateCard", this.card);
       window.location.reload();
      }
    },
    cancel() {
         window.location.reload();
    }
  },
  computed: {
    selected() {
      return this.card.selected;
    }
  }
};
</script>



<style lang="scss" scoped>
.button {
  margin-right: 10px;
  background-color: #000000;
  border-radius: 8px;
  outline: none;
  width: 181px;
  height: 40px;
  border: none;
  font-family: "PT Mono";
  color: #ffffff;
  font-weight: bold;
  font-size: 16px;
  margin-top: 10px;
}

.buttons {
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
}

label {
  margin-bottom: -14px;
  font-family: "PT Mono";
  font-size: 10px;
  color: rgba(0, 0, 0, 0.8);
  text-transform: uppercase;
}

input {
  font-family: "PT Mono";
  margin: 14px 0;
  text-align: center;
  width: 382px;
  font-size: 15px;
  height: 40px;
  border-radius: 8px;
  border-color: rgba(0, 0, 0, 0.8);
  border-width: 1px;
  outline: none;
}

select {
  width: 382px;
  height: 42px;
  border-radius: 8px;
  border-color: rgba(0, 0, 0, 0.8);
  border-width: 1px;
  outline: none;
}

input::placeholder {
  font-size: 15px;
  font-family: "PT Mono";
  text-align: center;
}

.card_form {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  height: 200px;
   padding: 1rem;
   width: 382px;
}

.notValid {
  color: red;
}
</style>
