<template>
<section>
  <article
    v-if="this.cardNumber != undefined"
    @click="activeCard"
    class="card"
    :class="vendor"
  >
 
    <img class="chip_logo" src="../assets/chip-light.svg" alt />
    <img
      class="vendor_logo"
      v-if="imgPath"
      :src="require(`../assets/${imgPath}.svg`)"
      :alt="imgPath"
    />
    <p class="card_number">{{ cardNum }}</p>
    <p class="card_holder">Cardholder name</p>
    <p class="card_holder_value">{{ cardHolder }}</p>
    <p class="valid_thru">Valid Thru</p>
    <p class="valid_thru_value">{{ validThru }}</p>
 
  </article>
</section>
</template>

<script>
export default {
  name: "card",
  data: () => {
    return {};
  },
  props: {
    cardNumber: String,
    cardHolder: String,
    validThru: String,
    cvvCode: String,
    imgPath: String
  },
  computed: {
    cardNum() {
      return this.cardNumber.toString().replace(/\d{4}(?=.)/g, "$& ");
    },
    vendor() {
      if (this.imgPath == "vendor-ninja") {
        return "vendor-ninja";
      } else if (this.imgPath == "vendor-bitcoin") {
        return "vendor-bitcoin";
      } else if (this.imgPath == "vendor-evil") {
        return "vendor-evil";
      } else if (this.imgPath == "vendor-blockchain") {
        return "vendor-blockchain";
      } else if (this.imgPath == "") {
        return "novendor";
      }
      return "";
    }
  },
  methods: {
    activeCard() {
      this.$emit("setActive");
    }
  }
};
</script>

<style lang="scss" scoped>
.card {
  position: relative;
  height: 200px;
  width: 382px;
  border-radius: 0.5rem;
  padding: 1rem;
  margin: 1px;
  background: linear-gradient(237.75deg, rgba(255, 255, 255, 0.24) 0%, rgba(255, 255, 255, 0) 100%), #D0D0D0;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.2);
}

.vendor-ninja {
  color: rgb(202, 202, 202);
  background-color: #222222;
}

.vendor-bitcoin {
  background-color: #ffae34;
}

.vendor-evil {
  color: rgb(202, 202, 202);
  background-color: #f33355;
}

.vendor-blockchain {
  background-color: #8b58f9;
}

.novendor {
  background-color: #d0d0d0;
}

.chip_logo {
  position: absolute;
  top: 23px;
  left: 23px;
}

.vendor_logo {
  position: absolute;
  top: 30px;
  right: 23px;
}

.card_holder {
  font-family: "PT Mono";
  font-size: 12px;
  text-transform: uppercase;
  position: absolute;
  bottom: 30px;
  left: 16px;
}

.card_holder_value {
  font-family: "PT Mono";
  font-size: 15px;
  position: absolute;
  bottom: 0px;
  left: 16px;
}

.valid_thru {
  font-family: "PT Mono";
  font-size: 12px;
  text-transform: uppercase;
  position: absolute;
  bottom: 30px;
  right: 16px;
}

.valid_thru_value {
  font-family: "PT Mono";
  font-size: 15px;
  position: absolute;
  bottom: 0px;
  right: 16px;
}

.card_number {
  margin-top: 18px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
  font-family: "PT Mono";
  font-size: 25px;
}
</style>
