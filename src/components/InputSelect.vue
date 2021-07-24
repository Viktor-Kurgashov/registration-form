<template>
  <div class="input-select fieldset-item">
    <label class="label_inline">
      {{ label }}
      <span v-if="required">*</span>
    </label>

    <div class="custom-select">
      <span class="custom-select__title"
        :class="{ 'custom-select__title_active': selectListOpened, 'incorrect-field-highlight': requiredSelectIsEmpty }"
        @click="this.selectListOpened = true">{{ title }}
      </span>

      <div class="custom-select__list" v-show="selectListOpened">
        <div class="custom-select__option" v-for="option of options" :key="option.id">
          <input type="radio"
            class="removed-checkbox"
            :name="name"
            :value="option.title"
            :id="option.id"
            @change="toggleOption">

          <label :for="option.id" class="custom-select__label">{{ option.title }}</label>
        </div>        
      </div>

      <div v-show="selectListOpened"
        class="modal-background"
        @click="this.selectListOpened = false">
      </div>
    </div>

    <div class="tooltip" v-if="requiredSelectIsEmpty">
      <span class="tooltip__body">Обязательное поле</span>
      <span class="tooltip__arrow clearfix"></span>
    </div>

  </div>
</template>

<script>
export default {
  name: 'InputSelect',

  methods: {
    toggleOption(event) {
      this.$emit('changed', {name: this.name, isCorrect: true} );
      this.title = event.target.value;
      this.visited = true;
    }
  },
  data() {
    return {
      selectListOpened: false,
      visited: undefined,
      title: "",
    };
  },
  computed: {
    requiredSelectIsEmpty() {
      if (!this.required) {
        return false;
      } else {
        if (this.incorrectFieldNotification) {
          return (!this.visited) ? true : false;
        }
      }
    }
  },
  props: {
    label: String,
    name: String,
    options: Array,
    required: Boolean,
    incorrectFieldNotification: Boolean,
  }
}
</script>