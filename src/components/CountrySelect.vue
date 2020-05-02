<template>
  <select :class="selectorClass" class="phone_input__select" v-model="code">
    <option
      v-for="country in countries"
      :key="country.code"
      :value="country.code"
    >
      {{ country.name }}
    </option>
  </select>
</template>

<script>
import translations from "@/translations";

export default {
  props: {
    value: {
      type: String,
      default: "sv"
    },
    locale: {
      type: String,
      default: "es"
    },
    selectorClass: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      code: null
    };
  },
  watch: {
    value(code) {
      if (code) {
        this.code = code;
        this.$emit("input", code);
      }
    },
    code(value) {
      if (value) this.$emit("input", value);
    }
  },
  computed: {
    countries() {
      const countries = translations[this.locale].countries;
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
