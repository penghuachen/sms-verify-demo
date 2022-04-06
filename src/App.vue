<template>
  <div id="app">
    <h1>簡訊驗證碼元件</h1>
    <input
      type="text"
      v-for="(value, index) in SMS"
      :key="index"
      :ref="index"
      :value="value"
      maxlength="1"
      @input="updateSMS($event, index)"
      @click="selectedInputValue(index)"
      @keyup="autoJumpNextInput($event, index)"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      SMS: [null, null, null, null]
    };
  },
  methods: {
    selectedInputValue(index) {
      const [input] = this.$refs[index];
      input.focus();
      input.select();
    },
    autoJumpNextInput(e, index) {
      if (index + 1 === this.SMS.length) return;

      const isValidValue = this.checkInputValue(e.target.value);

      let [input] =
        index + 1 >= this.SMS.length || !isValidValue
          ? this.$refs[index]
          : this.$refs[index + 1];

      if (index + 1 >= this.SMS.length || isValidValue) {
        input.focus();
        input.select();
      }
    },
    updateSMS(e, index) {
      const isValidValue = this.checkInputValue(e.target.value);

      !isValidValue
        ? (this.SMS[index] = null)
        : (this.SMS[index] = isValidValue);
      this.SMS = [...this.SMS];
    },
    checkInputValue(value) {
      return value.replace(/[^\d]/g, "");
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

input[type="text"] {
  width: 50px;
  height: 45px;
  margin-right: 14px;
  border-radius: 5px;
  border: 1px solid #bbbbbb;
  color: #808080;
  outline: none;
  font-size: 22px;
  text-align: center;
  &::-webkit-inner-spin-button,
  &::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  &:last-child {
    margin-right: 0;
  }
}
</style>
