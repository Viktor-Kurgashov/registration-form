<template>
  <div class="registration-notice">
    <span>Cообщение о том, что новый клиент успешно создан</span>

    <div class="return-button" @click="this.$emit('returnToForm')">

      <span class="return-button__arrow">&#129044;</span>
      <span>Назад</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RegistrationNotice',
}
</script>

<style>
.registration-notice > span {
  background: rgb(83, 198, 135);
  border-radius: 0.4em;
  text-align: center;
  color: black;

  font-size: 1.5rem;
  display: block;
  padding: 1em 0;
}

.return-button {
  margin-top: 1em;
  border-radius: 0.5em;
  background-color: dimgrey;
  color: whitesmoke;
  cursor: pointer;

  display: flex;
  justify-content: center;
  align-items: center;
}
.return-button:hover {
  background-color: grey;
}
.return-button__arrow {
  font-size: 2em;
  line-height: 1em;
  margin-right: 0.2em;
}
</style>