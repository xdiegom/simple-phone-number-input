<template>
  <div class="phone_input__container">
    <label class="phone_input__label">Phone number</label>
    <div class="phone_input__input">
      <select class="phone_input__select" v-model="countryCode">
        <option
          v-for="country in countries"
          :key="country.code"
          :value="country.code"
        >
          {{ country.name }}
        </option>
      </select>
      <span class="phone_input__dial_code">+503</span>
      <input v-model="phoneNumberInput" type="text" />
    </div>
  </div>
</template>

<script>
import allCountries from "../countries";
export default {
  name: "PhoneNumberInput",
  props: {
    locale: {
      type: String,
      default: "es"
    },
    defaultCountryCode: {
      type: String,
      default: "sv"
    }
  },
  data() {
    return {
      countryCode: null,
      phoneNumberInput: null
    };
  },
  mounted() {
    this.countryCode = this.defaultCountryCode;
  },
  computed: {
    countries() {
      const countries = allCountries[this.locale];
      return Object.keys(countries).map(code => {
        return {
          code: code,
          name: countries[code]
        };
      });
    }
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

.phone_input__select {
  max-width: 10rem;
}
</style>
