<template>
  <div class="input-select-multiple fieldset-item">
    <label class="label_inline">{{ label }} *</label>

    <div class="custom-select">
      <input type="text"
        readonly
        class="custom-select__title"
        :class="{ 'custom-select__title_active': selectListOpened, 'incorrect-field-highlight': incorrectField }"
        :name="name"
        :value="title"
        @click="this.selectListOpened = true;">
      
      <div v-show="selectListOpened" class="custom-select__list">
        <div class="custom-select__option" v-for="option of options" :key="option.id">
          <input type="checkbox"
            class="removed-checkbox"
            :value="option.title"
            :id="option.id"
            @change="collectOptions">

          <label :for="option.id" class="custom-select__label">{{ option.title }}</label>
        </div>
      </div>

      <div v-show="selectListOpened"
        class="modal-background"
        @click="this.selectListOpened = false">
      </div>    
    </div>

    <div class="tooltip" v-if="incorrectField">
      <span class="tooltip__body">Обязательное поле</span>
      <span class="tooltip__arrow clearfix"></span>
    </div>

  </div>
</template>

<script>
export default {
  name: 'InputSelectMultiple',

  methods: {
    collectOptions(event) {
      if (event.target.checked) {
        if (this.selected.length === 0) {
          this.atLeastOneSelected = true;
          this.$emit('changed', {name: this.name, isCorrect: true} )
        }
        this.selected.push(event.target.value);
      } else {
        this.selected.splice(this.selected.indexOf(event.target.value), 1);
        if (this.selected.length === 0) {
          this.atLeastOneSelected = false;
          this.$emit('changed', {name: this.name, isCorrect: false} );
        }
      }
    }
  },
  data() {
    return {
      selectListOpened: false,
      selected: [],
      atLeastOneSelected: undefined
    }
  },
  computed: {
    title() {
      return this.selected.join(", ");
    },
    incorrectField() {
      if (this.incorrectFieldNotification) {
        return (this.atLeastOneSelected) ? false : true;
      } else {
        return (this.atLeastOneSelected === false);
      }
    }
  },
  props: {
    label: String,
    name: String,
    options: Array,
    incorrectFieldNotification: Boolean
  }
}
</script>

<style>
.custom-select {
  position: relative;
  height: 2em;
  line-height: 2em;
}
.custom-select__title {
  background-color: FloralWhite;
  border: 1px solid darkgrey;
  border-radius: 0.2em;
  outline: none;

  font-size: inherit;
  display: block;
  width: 100%;
  height: 2em;
  padding: 0 0.4em;
}
.custom-select__title_active {
  border-color: dimgrey;
  border-radius: 0;
}
.modal-background {
  position: fixed;
  z-index: 10;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
.custom-select__list {
  background-color: rgb(255, 239, 204);
  border: 1px solid darkgrey;
  border-top: none;
  position: absolute;
  z-index: 20;
  width: 100%;
}
.custom-select__option {
  border-bottom: 1px solid lightgrey;
  position: relative;
  height: 2em;
}
.custom-select__option:last-child {
  border-bottom: none;
}
.custom-select__label {
  cursor: pointer;
  display: block;
  height: 100%;
  padding: 0 0.5em;
}
.removed-checkbox:checked + .custom-select__label {
  box-shadow: inset 0 0 0.5em rgb(160, 140, 75);
}
</style>