<template>
  <div class="phone_input__container">
    <label class="phone_input__label">
      <slot name="label">{{ labelTitle }}</slot>
    </label>
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
      <span class="phone_input__dial_code"> +{{ countryCallingCode }} </span>
      <input
        v-model="phoneNumberInput"
        type="text"
        v-mask="mask"
        :placeholder="mask"
      />
    </div>
  </div>
</template>

<script>
import {
  getExampleNumber,
  parsePhoneNumberFromString
} from "libphonenumber-js";
import examples from "libphonenumber-js/examples.mobile.json";
import translations from "../translations";
import { VueMaskDirective } from "v-mask";

export default {
  name: "PhoneNumberInput",
  directives: {
    mask: VueMaskDirective
  },
  props: {
    value: {
      type: String,
      default: null
    },
    locale: {
      type: String,
      default: "es",
      validator: function(value) {
        if (Object.keys(translations).indexOf(value) === -1) {
          console.error(`Locale ${value} is not supported`);
          return false;
        }
        return true;
      }
    },
    defaultCountryCode: {
      type: String,
      default: "sv",
      validator: function(value) {
        const { es } = translations;
        if (Object.keys(es.countries).indexOf(value) === -1) {
          console.error(`Country code '${value}' is not supported`);
          return false;
        }
        return true;
      }
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
  watch: {
    value(number) {
      if (number) this.phoneNumberInput = number;
    },
    phoneNumberInput() {
      this.format();
    },
    countryCode() {
      this.format();
    }
  },
  computed: {
    labelTitle() {
      return translations[this.locale].label;
    },
    countries() {
      const countries = translations[this.locale].countries;
      return Object.keys(countries).map(code => {
        return {
          code: code,
          name: countries[code]
        };
      });
    },
    countryCallingCode() {
      if (this.phoneNumberExample)
        return this.phoneNumberExample.countryCallingCode;

      return "";
    },
    phoneNumberExample() {
      const phoneNumber = this.countryCode
        ? getExampleNumber(this.countryCode.toUpperCase(), examples)
        : null;
      return phoneNumber;
    },
    mask() {
      const internationalPhoneNumberExample = this.phoneNumberExample
        ? this.phoneNumberExample.formatInternational()
        : null;
      const nationalPhoneNumberExample = this.phoneNumberExample
        ? this.phoneNumberExample.formatNational()
        : null;

      const phonePartMask = internationalPhoneNumberExample
        ? internationalPhoneNumberExample.split(" ")
        : [];

      const mask =
        phonePartMask.length > 1
          ? nationalPhoneNumberExample.replace(/\d/g, "#")
          : "";

      return mask;
    }
  },
  methods: {
    format() {
      const number = this.phoneNumberInput;
      if (number) {
        const phoneNumber = parsePhoneNumberFromString(
          number,
          this.countryCode.toUpperCase()
        );
        if (phoneNumber) {
          if (phoneNumber.country !== this.countryCode) {
            this.countryCode = phoneNumber.country.toLowerCase();
          }
          const formatted = {
            phoneNumberExample: this.phoneNumberExample.nationalNumber,
            countryCode: phoneNumber.country,
            countryCallingCode: phoneNumber.countryCallingCode,
            nationalNumber: phoneNumber.nationalNumber,
            internationalNumber: phoneNumber.number,
            isValid: phoneNumber.isValid()
          };
          this.$emit("onFormatted", formatted);
        }
      } else this.$emit("onFormatted", null);
      this.$emit("input", number);
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
