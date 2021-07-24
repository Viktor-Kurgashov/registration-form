<template>
  <div class="input-phone-number fieldset-item">
    <label class="label_inline">Номер телефона *</label>

    <input type="text"
      inputmode="numeric"
      class="input-text__text-field"
      :class="{ 'incorrect-field-highlight': incorrectField }"
      placeholder="+7"
      maxlength="16"
      v-model="value"
      @focus="addCountryCode"
      @keydown="backspaceFix"
      @input="addSpaces"
      @blur="validate" />

    <input type="hidden" name="phoneNumber" v-model="editedNumber">

    <div class="tooltip" v-if="incorrectField">
      <span class="tooltip__body">Неправильный номер</span>
      <span class="tooltip__arrow clearfix"></span>
    </div>

  </div>
</template>

<script>
export default {
  name: 'InputPhoneNumber',
  methods: {
    addCountryCode() {
      if (!/^\+7 /.test(this.value)) this.value = "+7 ";
    },
    backspaceFix(event) {
      if (event.key === "Backspace") {
        event.preventDefault();
        this.value = this.value.replace(/. ?$/, "");
      }
    },
    addSpaces() {
      if (this.value.length === 6  ||
          this.value.length === 10 ||
          this.value.length === 13 ||
          /\+7$/.test(this.value) )  { this.value += " "; }
    },
    validate(event) {
      if (/\+7 ?\d{3} ?\d{3} ?\d{2} ?\d{2}/.test(this.value)) {
        this.validationResult = true;
        this.$emit('changed', {name: 'phoneNumber', isCorrect: true} );
      } else {
        this.validationResult = false;
        this.$emit('changed', {name: 'phoneNumber', isCorrect: false} );
      }
    },
  },
  data() {
    return {
      value: "",
      validationResult: undefined
    };
  },
  computed: {
    editedNumber() {
      return this.value.replace(/[^\d]/g, "");
    },
    incorrectField() {
      if (this.incorrectFieldNotification) {
        return (this.validationResult) ? false : true;
      } else {
        return (this.validationResult === false);
      }
    }
  },
  props: ['incorrectFieldNotification']
}
</script>