<template>
  <div id="app">
    <h1>簡訊驗證碼元件</h1>
    <input 
      type="text"
      v-for="(value, index) in smsValue" :key="index"
      :ref="index"
      :value="value"
      maxlength="1"
      @input="updateSMSValue($event, index)"
      @click="selectedInputValue(index)"
      @keydown="autoTriggerInput($event, index)"
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      smsValue: [null, null, null, null],
    }
  },
  methods: {
    selectedInputValue(index) {
      const [input] = this.$refs[index];
      input.select();
    },
    autoTriggerInput(e, index) {
      console.log('key');
      const isWantedValue = this.checkInputValue(e.target.value);

      if (index + 1 >= this.smsValue.length || !isWantedValue) {
        const [input] = this.$refs[index];
        input.select();
      } else {
        const [input] = this.$refs[index + 1];
        input.select(); 
      }
    },
    updateSMSValue(e, index) {
      console.log('input');
      const isWantedValue = this.checkInputValue(e.target.value);

      !isWantedValue 
        ? this.smsValue[index] = null
        : this.smsValue[index] = Number(isWantedValue);
      this.smsValue = [...this.smsValue];
    },
    checkInputValue(value) {
      return value.replace(/[^\d]/g, '');
    }
  }
}
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
