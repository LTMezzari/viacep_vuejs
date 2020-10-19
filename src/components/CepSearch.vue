<template>
  <div>
    <div class="SearchView">
      <input
        v-model="text"
        type="text"
        placeholder="Type the cep here..."
        class="SearchInput"
      />
      <button v-on:click="send" class="MaterialButton SearchButton">
        Enviar
      </button>
    </div>
    <transition-group name="fade" tag="ul" class="Results">
      <li v-for="(item, index) in result" v-bind:key="index">
        <div class="Row">
          <h3>
            {{
              `${item.logradouro}, ${item.bairro} - ${item.localidade} / ${item.uf} (${item.cep})`
            }}
          </h3>
          <button v-on:click="add(item)" class="MaterialButton">
            Adicionar
          </button>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CepSearch",
  props: {
    didPressAdd: {
      type: Function,
      required: false,
    },
  },
  data: function () {
    return {
      text: "",
      result: [],
    };
  },
  methods: {
    send: function () {
      axios
        .get(`https://viacep.com.br/ws/${this.text}/json`)
        .then((r) => {
            if (!r.data.erro) {
                this.result = [r.data]
            }
        })
        .catch((e) => console.log(e));
    },
    add: function (item) {
      if (this.didPressAdd) this.didPressAdd(item);

      this.text = "";
      this.result = [];
    },
  },
};
</script>

<style scoped>
/* Input Div */
.SearchView {
  width: 100%;
  display: flex;
}
/* Input */
.SearchInput {
  flex: 3;
  padding: 1.5em 1em;
  font-size: 1em;
  outline: 0;
  border: none;
  border-radius: 4px 0px 0px 4px;
  box-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 1px 5px 0 rgba(0, 0, 0, 0.12);
}
/* Results */
.Results {
  margin: 0;
  padding: 0;
  text-align: left;
  position: relative;
}
/* Result Items */
.Results li {
  background: rgba(53, 73, 94, 0.3);
  margin: 0;
  list-style: none;
  width: 100%;
  border-bottom: 1px solid #394e62;
  transition: ease-in-out 0.5s;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-ender,
.fade-leave-to {
  opacity: 0;
}
/* Material Button */
.MaterialButton {
  position: relative;
  display: inline-block;
  box-sizing: border-box;
  border: none;
  border-radius: 4px;
  padding: 0 16px;
  min-width: 64px;
  height: 36px;
  vertical-align: middle;
  text-align: center;
  text-overflow: ellipsis;
  text-transform: uppercase;
  color: rgb(var(--pure-material-onprimary-rgb, 255, 255, 255));
  background-color: rgb(var(--pure-material-primary-rgb, 33, 150, 243));
  box-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 1px 5px 0 rgba(0, 0, 0, 0.12);
  font-family: var(
    --pure-material-font,
    "Roboto",
    "Segoe UI",
    BlinkMacSystemFont,
    system-ui,
    -apple-system
  );
  font-size: 14px;
  font-weight: 500;
  line-height: 36px;
  overflow: hidden;
  outline: none;
  cursor: pointer;
  transition: box-shadow 0.2s;
}
.MaterialButton::-moz-focus-inner {
  border: none;
}
.MaterialButton::before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgb(var(--pure-material-onprimary-rgb, 255, 255, 255));
  opacity: 0;
  transition: opacity 0.2s;
}
.MaterialButton::after {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  border-radius: 50%;
  padding: 50%;
  width: 32px; /* Safari */
  height: 32px; /* Safari */
  background-color: rgb(var(--pure-material-onprimary-rgb, 255, 255, 255));
  opacity: 0;
  transform: translate(-50%, -50%) scale(1);
  transition: opacity 1s, transform 0.5s;
}
.MaterialButton:hover,
.MaterialButton:focus {
  box-shadow: 0 2px 4px -1px rgba(0, 0, 0, 0.2), 0 4px 5px 0 rgba(0, 0, 0, 0.14),
    0 1px 10px 0 rgba(0, 0, 0, 0.12);
}
.MaterialButton:hover::before {
  opacity: 0.08;
}
.MaterialButton:focus::before {
  opacity: 0.24;
}
.MaterialButton:hover:focus::before {
  opacity: 0.3;
}
.MaterialButton:active {
  box-shadow: 0 5px 5px -3px rgba(0, 0, 0, 0.2),
    0 8px 10px 1px rgba(0, 0, 0, 0.14), 0 3px 14px 2px rgba(0, 0, 0, 0.12);
}
.MaterialButton:active::after {
  opacity: 0.32;
  transform: translate(-50%, -50%) scale(0);
  transition: transform 0s;
}
.MaterialButton:disabled {
  color: rgba(var(--pure-material-onsurface-rgb, 0, 0, 0), 0.38);
  background-color: rgba(var(--pure-material-onsurface-rgb, 0, 0, 0), 0.12);
  box-shadow: none;
  cursor: initial;
}
.MaterialButton:disabled::before {
  opacity: 0;
}
.MaterialButton:disabled::after {
  opacity: 0;
}
/* Search Button */
.SearchButton {
  flex: 1;
  border-radius: 0px 4px 4px 0px;
  height: auto;
}
/* Cep Row Container */
.Row {
  display: flex;
  align-items: center;
  padding: 1em;
}
.Row h3 {
  flex: 6;
}
.Row button {
  flex: 1;
}
</style>