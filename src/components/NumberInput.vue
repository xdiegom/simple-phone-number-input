<template>
  <div
    class="phone_input__input"
    :class="{ phone_input__hidden_dial_code: noDialCode }"
  >
    <span v-show="!noDialCode" class="phone_input__dial_code">
      +{{ countryCallingCode }}
    </span>
    <input
      id="phone__input"
      :class="inputClass"
      v-model="phoneNumberInput"
      type="text"
      v-mask="mask"
      :placeholder="mask"
    />
  </div>
</template>

<script>
import { VueMaskDirective } from "v-mask";
import {
  getExampleNumber,
  parsePhoneNumberFromString
} from "libphonenumber-js";
import examples from "libphonenumber-js/examples.mobile.json";

export default {
  props: {
    value: {
      type: String,
      default: null
    },
    countryCode: {
      type: String,
      default: "sv"
    },
    noDialCode: {
      type: Boolean,
      default: false
    },
    inputClass: {
      type: String,
      default: null
    }
  },
  directives: {
    mask: VueMaskDirective
  },
  data() {
    return {
      phoneNumberInput: null,
      currentCountryCode: null
    };
  },
  watch: {
    phoneNumberInput() {
      this.format();
    },
    countryCode(code) {
      this.currentCountryCode = code;
    }
  },
  computed: {
    countryCallingCode() {
      if (this.phoneNumberExample)
        return this.phoneNumberExample.countryCallingCode;

      return "";
    },
    phoneNumberExample() {
      const phoneNumber = this.currentCountryCode
        ? getExampleNumber(this.currentCountryCode.toUpperCase(), examples)
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
          this.currentCountryCode.toUpperCase()
        );
        if (phoneNumber) {
          if (phoneNumber.country !== this.currentCountryCode) {
            this.currentCountryCode = phoneNumber.country.toLowerCase();
          }
          const formatted = {
            phoneNumberExample: this.phoneNumberExample.nationalNumber,
            countryCode: phoneNumber.country,
            countryCallingCode: phoneNumber.countryCallingCode,
            nationalNumber: phoneNumber.nationalNumber,
            internationalNumber: phoneNumber.number,
            isValid: phoneNumber.isValid()
          };
          this.$emit("onInput", formatted);
        }
      } else this.$emit("onInput", null);
      this.$emit("input", number);
    }
  }
};
</script>

<style scoped>
#phone__input {
  width: 100%;
}
</style>
