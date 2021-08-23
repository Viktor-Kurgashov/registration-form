<template>
  <div class="input-text fieldset-item">
    <label :for="id" class="label_inline">
      {{ label }}
      <span v-if="required">*</span>
    </label>

    <template v-if="required">
      <input type="text"
      class="input-text__text-field"
      :class="{ 'incorrect-field-highlight': incorrectField }"
      :name="name"
      :id="id"
      @blur="validate" />

      <div class="tooltip" v-if="incorrectField">
        <span class="tooltip__body">{{ tooltip }}</span>
        <span class="tooltip__arrow clearfix"></span>
      </div>
    </template>

    <input v-else
      type="text"
      class="input-text__text-field"
      :name="name"
      :id="id" />
  </div>
</template>

<script>
export default {
  name: 'InputText',

  methods: {
    validate(event) {
      if (event.target.value.trim() === "") {
        this.validationResult = false;
        this.$emit('changed', {name: event.target.name, isCorrect: false} );
      } else {
        this.validationResult = true;
        this.$emit('changed', {name: event.target.name, isCorrect: true} );
      }
    }
  },
  data() {
    return {
      validationResult: undefined
    }
  },
  computed: {
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
    id: String,
    name: String,
    required: Boolean,
    tooltip: String,
    incorrectFieldNotification: Boolean
  },
  inheritAttrs: false,
}
</script>

<style>
.input-text__text-field {
  font-size: inherit;
  display: block;
  width: 100%;
  height: 2em;
  padding: 0 0.4em;

  background-color: FloralWhite;
  border: 1px solid DarkGrey;
  border-radius: 0.2em;
  outline: none;
}
.input-text__text-field:focus {
  background-color: rgb(255, 239, 204);
  border-color: DimGray;
}
</style>