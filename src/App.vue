<template>
  <div id="app" class="Container">
    <div class="Search">
      <h1 class="text-center">Bem Vindo ao carrinho de CEP's</h1>
      <CepSearch :didPressAdd="add" class="Wrap"/>
    </div>
    <CepCart :cart="cart" :didPressRemove="remove" class="Cart"/>
  </div>
</template>

<script>
import CepSearch from "./components/CepSearch.vue";
import CepCart from "./components/CepCart.vue";

export default {
  name: "App",
  data: function () {
    return {
      cart: [],
    };
  },
  components: {
    CepSearch,
    CepCart,
  },
  methods: {
    add: function (item) {
      item.id = this.cart.length;
      this.cart.push(item);
    },
    remove: function (index) {
      this.cart.splice(index, 1);
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #41b883 0%, #354953 100%);
  min-height: 100vh;
  box-sizing: border-box;
  background-size: cover;
}
#app {
  font-family: "Open Sans", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
h1 {
  font-size: 3vw;
  margin: 0;
  padding: 0;
  font-family: "Lobster", Arial, sans-serif;
}
.text-center {
  text-align: center;
}
.Wrap {
  max-width: 60vw;
  margin: 15vh auto;
}
.Container {
  display: flex;
  min-height: 100vh;
  min-width: 100vh;
  box-sizing: border-box;
  background-size: cover;
  align-items: center;
}
.Search {
  flex: 3;
}
.Cart {
  flex: 1;
  background-color: #0000000F;
  min-height: 100vh;
}
</style>
