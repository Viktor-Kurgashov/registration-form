<template>
  <div class="input-radio fieldset-item">
    <span class="custom-radio-label">Пол</span>

    <label class="custom-radio">
      <input type="radio"
        class="removed-checkbox"
        name="gender"
        value="male">

      <div class="custom-radio__checkmark-field">
        <span class="custom-radio__checkmark-dot"></span>
      </div>

      <span>Мужской</span>
    </label>

    <label class="custom-radio">
      <input type="radio"
        class="removed-checkbox"
        name="gender"
        value="female">

      <div class="custom-radio__checkmark-field">
        <span class="custom-radio__checkmark-dot"></span>
      </div>

      <span>Женский</span>
    </label>

  </div>
</template>

<script>
export default {
  name: "CustomRadio"
}
</script>

<style>
.custom-radio-label {
  display: block;
  padding: 0.3em 0.3em 0.1em;
}
.custom-radio {
  position: relative;
  display: inline-flex;
  align-items: center;
  padding: 0 0.3em;
  height: 2em;
  cursor: pointer;
}
.custom-radio__checkmark-field {
  background-color: lightgrey;
  margin-right: 0.4em;
  border-radius: 50%;
  position: relative;
  display: inline-block;
  width: 1.2em;
  height: 1.2em;
}
.custom-radio__checkmark-dot {
  background-color: inherit;
  border-radius: 50%;
  display: inline-block;
  width: 40%;
  height: 40%;
  position: absolute;
  left: 30%;
  top: 30%;
}
.removed-checkbox:checked + .custom-radio__checkmark-field {
  background-color: dimgrey;
}
.removed-checkbox:checked + .custom-radio__checkmark-field > .custom-radio__checkmark-dot {
  background-color: ghostwhite;
}
</style>