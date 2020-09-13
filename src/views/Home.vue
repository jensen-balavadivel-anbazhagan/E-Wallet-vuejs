<template>
  <section class="show_wallet">
    <top :pageTitle="'E-Wallet'" :subTitle="'Active Card'" />
    <card
      class="active_card"
      :cardNumber="activeCard.cardNumber"
      :cardHolder="activeCard.cardHolder"
      :imgPath="activeCard.selected"
      :validThru="activeCard.validThru"
      :cvvCode="activeCard.cvvCode"
    />
    <cardStack
      :cards="cards"
      @card="setActive"
      :active="activeCard"
      @resetActive="resetActive"
    />
  </section>
</template>

<script>
import top from "../components/Top";
import card from "../components/Card";
import cardStack from "../components/CardStack";

export default {
  name: "home",
  components: {
    top,
    cardStack,
    card
  },
  data: () => {
    return {
      activeCard: {},
      cards: JSON.parse(localStorage.getItem("cards"))
    };
  },
  methods: {
    setActive(value) {
      this.activeCard = value[0];
      return this.activeCard;
    },
    resetActive() {
      this.activeCard = {};
    }
  }
};
</script>

<style lang="scss" scoped>


.show_wallet {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}

.active_card {
  transform: translateY(-50px);
}
</style>
