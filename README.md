# simple-phone-number-input

[![vue 2](https://img.shields.io/badge/vue-2-42b983.svg?style=flat-square)](https://vuejs.org)
[![license](https://img.shields.io/github/license/LouisMazel/vue-phone-number-input.svg?style=flat-square)](https://github.com/xdiegom/simple-phone-number-input/blob/master/LICENSE)

> Simple input field to format international phone numbers inspired by [VuePhoneNumberInput](https://github.com/LouisMazel/vue-phone-number-input). 
> Made with VueJS 💚

## 📦 Third party packages 

### ✔ [v-mask 💚](https://github.com/probil/v-mask)
### ✔ [libphonenumber-js](https://github.com/catamphetamine/libphonenumber-js)

## Installation

### Using yarn

`yarn add simple-phone-number-input`

### Using npm

`npm i --save simple-phone-number-input`

## Usage

### ES6 Modules / CommonJS

```js
import SimplePhoneNumberInput from "simple-phone-number-input";
import "simple-phone-number-input/dist/simple-phone-number-input.css";

Vue.use(SimplePhoneNumberInput);
```

```html
<simple-phone-number-input v-model="yourValue" />
```

## Props API

| Props                     | Type            | Required | Default             |
|---------------------------|-----------------|----------|---------------------|
| v-model                   | String          | true     | -                   |
| locale                    | String          | false    |'es'                 |
| default-country-code      | String          | false    |'sv'                 |
| input-class  *            | String          | false    |-                    |
| selector-class  *         | String          | false    |-                    |
| no-dial-code              | Boolean         | false    |false                |
| no-country-selector       | Boolean         | false    |false                |

\* These properties are used to apply custom or framework css classes.

## Events API

| Event                | Return                                                                                                                                                                            |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| onFormatted | `-` emitted when new value is enter on phone number input and return an object of different formatted phone number values (1)                                                                                 
(1) - Example of object returned when *onFormatted* event is emitted:
```
{
  phoneNumberExample: "70123456",
  countryCode: "SV",
  countryCallingCode: "503",
  nationalNumber: "72665487",
  internationalNumber: "+50372665487",
  isValid: true
}
```

### Object properties definition

| Property  | Type | Description | 
|-----------|-------------|-------------|
| phoneNumberExample  |  String | Local phone number example of the selected country
| countryCode         | String  | Selected country code [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISOString_3166-1_alpha-2)
| countryCallingCode  |  String | Selected country calling code
| nationalNumber      |  String | Formatted national phone number
| internationalNumber |  String | Formatted international phone number
| isValid             |  Boolean | Validation result of the current phone number. Check libphonenumber-js for more info: [isValid()](https://github.com/catamphetamine/libphonenumber-js#isvalid-boolean)


## Named slots

| Slot  | Action                                                                  |
|-------|-------------------------------------------------------------------------|
| label | Override the current label for the phone number input. Supported languages: spanish and english. |

## ✌ Contributing

If you have any suggestions, problems or ideas feel free to add a new [issue](https://github.com/xdiegom/simple-phone-number-input)

### Project setup

```bash
npm install
```

### Compiles and hot-reloads for development

```bash
npm run serve
```

### Lints and fixes files

```bash
npm run lint
```

## License

This project is licensed under [MIT License](http://en.wikipedia.org/wiki/MIT_License)