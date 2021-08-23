<template>
  <div class="input-date fieldset-item">
    <span class="label_inline" onclick="this.nextElementSibling.firstElementChild.focus()">
      {{ label }}
      <span v-if="required">*</span>
    </span>

    <div class="complex-date"
      :class="{ 'incorrect-field-highlight': incorrectField, 'complex-date_focus': complexDate_focus }">

      <input type="text"
        id="complex-date__day"
        inputmode="numeric"
        placeholder="31"
        maxlength="2"
        v-model="day"
        @focus="addFocusStyle"
        @input="dayOnInput"
        @blur="dayOnBlur(), removeFocusStyle()" />

      <span>.</span>

      <input type="text"
        class="complex-date__month"
        inputmode="numeric"
        placeholder="12"
        maxlength="2"
        v-model="month"
        @focus="addFocusStyle"
        @input="monthOnInput"
        @blur="monthOnBlur(), removeFocusStyle()"
        @keydown="monthBackspaceFix" />

      <span>.</span>

      <input type="text"
        class="complex-date__year"
        inputmode="numeric"
        placeholder="2000"
        maxlength="4"
        v-model="year"
        @focus="addFocusStyle"
        @input="yearOnInput"
        @blur="validate(), removeFocusStyle()"
        @keydown="yearBackspaceFix" />

      <input type="hidden" :name="name" v-model="fullDate" />
    </div>

    <div class="tooltip" v-if="incorrectField">
      <span class="tooltip__body">{{ tooltip }}</span>
      <span class="tooltip__arrow clearfix"></span>
    </div>

  </div>
</template>

<script>
export default {
  name: 'InputDate',

  data() {
    return {
      day: "",
      month: "",
      year: "",
      validationResult: undefined,
      complexDate_focus: false,
    };
  },
  computed: {
    fullDate() {
      return `${this.day}-${this.month}-${this.year}`;
    },
    incorrectField() {
      if (this.incorrectFieldNotification) {
        return (this.validationResult) ? false : true;
      } else {
        return (this.validationResult === false);
      }
    }
  },
  props: {
    label: String,
    name: String,
    required: Boolean,
    tooltip: String,
    incorrectFieldNotification: Boolean
  },

  methods: {
    addFocusStyle() {
      this.complexDate_focus = true;
    },
    removeFocusStyle() {
      this.complexDate_focus = false;
    },

    dayOnInput(event) {
      if (/^[4-9]/.test(this.day)) this.day = "0" + this.day;
      if (this.day.length === 2) event.target.nextElementSibling.nextElementSibling.focus();
    },
    dayOnBlur() {
      if (/^[1-3]$/.test(this.day.trim() )) this.day = "0" + this.day.trim();
      if (+this.day > 31) this.day = 31;
    },
  
    monthOnInput(event) {
      if ((this.month.length > 0) && !this.month.startsWith('1')) this.month = "0" + this.month;
      if (this.month.length === 2) event.target.nextElementSibling.nextElementSibling.focus();
    },
    monthOnBlur() {
      if (+this.month > 12) this.month = 12;
    },
    monthBackspaceFix(event) {
      if (event.key === "Backspace") {
        if (this.month.length === 0) {
          if (this.day.startsWith('0')) this.day = "";
          event.target.previousElementSibling.previousElementSibling.focus();
        } else if (this.month.startsWith('0')) {
          this.month = "";
        }
      }
    },
  
    yearOnInput() {
      if (/^1$/.test(this.year.trim())) {
        this.year = "19";
      } else if (/^2$/.test(this.year.trim())) {
        this.year = "20";
      }
    },
    validate() {
      if (/\d{2}-\d{2}-\d{4}/.test(this.fullDate)) {
        this.validationResult = true;
        this.$emit('changed', { name: this.name, isCorrect: true} );
      } else {
        this.validationResult = false;
        this.$emit('changed', { name: this.name, isCorrect: false} );
      }
    },
    yearBackspaceFix(event) {
      if (event.key === "Backspace") {
        if (this.year.length === 0) {
          if (this.month.startsWith('0')) this.month = "";
          event.target.previousElementSibling.previousElementSibling.focus();          
        } else if (this.year.length === 2) {
          this.year = "";
        }
      }
    },
  }
}
</script>

<style>
.complex-date {
  background-color: FloralWhite;
  border: 1px solid DarkGrey;
  border-radius: 0.2em;
  height: 2em;
  display: flex;
}
.complex-date > input {
  background-color: inherit;
  border: none;
  outline: none;
  padding: 0;
  text-align: center;
  font-size: inherit;
  flex-basis: 33%;
  width: 33%;
}
.complex-date > span {
  align-self: center;
}
#complex-date__day {
  border-radius: 0.2em 0 0 0.2em;
}
.complex-date__year {
  border-radius: 0 0.2em 0.2em 0;
}
.complex-date_focus {
  background-color: rgb(255, 239, 204);
  border-color: DimGray;
}
</style> 