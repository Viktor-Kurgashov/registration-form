<template>
  <div class="custom-checkbox fieldset-item">
    <input type="checkbox" class="removed-checkbox" :name="name" :id="chboxId">

    <label :for="chboxId" class="custom-checkbox__label">
      <div class="custom-checkbox__field">
        <span class="custom-checkbox__mark">&#x2714;</span>
      </div>

      <span>{{ label }}</span>
    </label>
  </div>
</template>

<script>
export default {
  name: 'InputCheckbox',
  props: ['label', 'chboxId', 'name'],
  inheritAttrs: false
}
</script>

<style>
.custom-checkbox {
  margin-top: 0.6em;
}
.custom-checkbox__label {
  cursor: pointer;
  user-select: none;
  display: inline-flex;
  align-items: center;
  height: 2em;
  padding: 0 0.3em;
}
.custom-checkbox__field {
  background-color: lightgrey;
  border-radius: 0.1em;
  display: inline-block;
  width: 1.2em;
  height: 1.2em;
  line-height: 1.2em;
  text-align: center;
  margin-right: 0.4em;
}
.removed-checkbox:checked + .custom-checkbox__label > .custom-checkbox__field {
  background-color: dimgrey;
}
.custom-checkbox__mark {
  visibility: hidden;
  color: ghostWhite;
}
.removed-checkbox:checked + .custom-checkbox__label > .custom-checkbox__field > .custom-checkbox__mark {
  visibility: visible;
}
</style>