<template>
  <section class="add_card">
    <top :pageTitle="'Add a new bank card'" :subTitle="'New Card'" />
    <card
      :validThru="validThru"
      :cardNumber="cardNumber"
      :cardHolder="cardHolder"
      :cvvCode="cvvCode"
      :imgPath="cardLogo"
    />
    <cardform
      :cvvValue="cvvCode"
      :validThruValue="validThru"
      :cardHolderValue="cardHolder"
      :cardNumberValue="cardNumber"
      @changeCvvValue="cvvCode = $event"
      @changeValidThru="validThru = $event"
      @changeCardNumber="cardNumber = $event"
      @changeCardHolder="cardHolder = $event"
      @cardLogo="cardLogo = $event"
      @updateCard="addCard"
    />
  </section>
</template>

<script>
import cardform from "../components/CardForm";
import top from "../components/Top";
import card from "../components/Card";

export default {
  name: "add-card",
  props: {},
  components: {
    top,
    card,
    cardform
  },
  data: () => {
    return {
      cardNumber: "",
      cardHolder: "",
      validThru: "",
      cvvCode: "",
      cardLogo: "",
      cardColor: "",
      cards: []
    };
  },
  created() {
    if (localStorage.getItem("cards") === null) {
      localStorage.setItem("cards", "[]");
    } else {
      this.cards = JSON.parse(localStorage.getItem("cards" || "[]"));
    }
  },
  methods: {
    addCard(value) {
      this.cards.push(value);
      localStorage.setItem("cards", JSON.stringify(this.cards));
      this.cardNumber = "";
      this.cardHolder = "";
      this.validThru = "";
      this.cvvCode = "";
      this.cardLogo = "";
      this.cardColor = "";
    }
  }
};
</script>
<style lang="scss" scoped>
.add_card {
  width: 382px;
}
</style>