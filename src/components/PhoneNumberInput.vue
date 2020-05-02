<template>
  <div class="phone_input__container">
    <label class="phone_input__label">Phone number</label>
    <div class="phone_input__input">
      <select v-model="countryCode">
        <option value="sv">El Salvador</option>
      </select>
      <span class="phone_input__dial_code">+503</span>
      <input v-model="phoneNumberInput" type="text" />
    </div>
  </div>
</template>

<script>
export default {
  name: "PhoneNumberInput",
  data() {
    return {
      countryCode: "sv",
      phoneNumberInput: null
    };
  }
};
</script>

<style scoped>
.phone_input__container {
  display: flex;
  flex-direction: column;
}

.phone_input__label {
  font-weight: bold;
  margin-bottom: 0.2rem;
}

.phone_input__input {
  display: flex;
  align-items: center;
}

.phone_input__dial_code {
  margin: 0 0.2rem;
}
</style>
