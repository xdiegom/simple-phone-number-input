<template>
  <div class="phone_input__container">
    <label class="phone_input__label">
      <slot name="label">{{ labelTitle }}</slot>
    </label>
    <div class="phone_input__input">
      <country-select
        v-if="!noCountrySelector"
        :selector-class="selectorClass"
        :locale="locale"
        v-model="countryCode"
      />
      <number-input
        :input-class="inputClass"
        :no-dial-code="noDialCode"
        :locale="locale"
        :country-code="countryCode"
        v-model="phoneNumberInput"
        @onInput="onFormat"
      />
    </div>
  </div>
</template>

<script>
import translations from "@/translations";
import CountrySelect from "./CountrySelect";
import NumberInput from "./NumberInput";

export default {
  name: "PhoneNumberInput",
  components: {
    CountrySelect,
    NumberInput
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
          // eslint-disable-next-line no-console
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
          // eslint-disable-next-line no-console
          console.error(`Country code '${value}' is not supported`);
          return false;
        }
        return true;
      }
    },
    noDialCode: {
      type: Boolean,
      default: false
    },
    inputClass: {
      type: String,
      default: null
    },
    selectorClass: {
      type: String,
      default: null
    },
    noCountrySelector: {
      type: Boolean,
      default: false
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
    phoneNumberInput(value) {
      this.$emit("input", value);
    }
  },
  computed: {
    labelTitle() {
      return translations[this.locale].label;
    }
  },
  methods: {
    onFormat(formatted) {
      if (formatted) {
        if (formatted.countryCode !== this.countryCode)
          this.countryCode = formatted.countryCode.toLowerCase();
      }
      this.$emit("onFormatted", formatted);
    }
  }
};
</script>

<style scoped>
@import "../assets/styles.css";
</style>
