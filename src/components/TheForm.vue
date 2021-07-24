<template>
  <form action="/" method="GET" @submit="verifyForm">
    <fieldset>
      <InputText
        label="Фамилия"
        id="lastname"
        name="lastname"
        required
        tooltip="Укажите фамилию"
        :incorrectFieldNotification="this.notifications__incorrect.lastname"
        @changed="collectStates" />

      <InputText
        label="Имя"
        id="firstname"
        name="firstname"
        required
        tooltip="Укажите имя"
        :incorrectFieldNotification="this.notifications__incorrect.firstname"
        @changed="collectStates" />

      <InputText
        label="Отчество"
        id="middlename"
        name="middlename" />

      <InputDate
        label="Дата рождения"
        name="birthday"
        required
        tooltip="Неправильная дата"
        :incorrectFieldNotification="this.notifications__incorrect.birthday"
        @changed="collectStates" />

      <InputPhoneNumber
        :incorrectFieldNotification="this.notifications__incorrect.phoneNumber"
        @changed="collectStates" />

      <CustomRadio />

      <InputSelectMultiple
        label="Группа клиентов"
        name="clientGroup"
        :options="groupOptions"
        :incorrectFieldNotification="this.notifications__incorrect.clientGroup"
        @changed="collectStates" />

      <InputSelect
        label="Лечащий врач"
        name="doctor"
        :options="doctorOptions" />

      <InputCheckbox
        label="Не отправлять СМС"
        chboxId="noSMS"
        name="noSMS" />
    </fieldset>

    <fieldset>
      <InputText
        label="Город"
        id="city"
        name="city"
        required
        tooltip="Обязательное поле"
        :incorrectFieldNotification="this.notifications__incorrect.city"
        @changed="collectStates" />

      <InputText
        label="Улица"
        id="street"
        name="street" />

      <InputText
        label="Дом"
        id="home"
        name="home" />
    </fieldset>

    <fieldset>
      <InputSelect
        label="Тип документа"
        name="documentType"
        :options="documentTypeOptions"
        required
        :incorrectFieldNotification="this.notifications__incorrect.documentType"
        @changed="collectStates" />

      <InputText
        label="Серия"
        id="documentSeries"
        name="documentSeries" />

      <InputText
        label="Номер"
        id="documentNumber"
        name="documentNumber" />

      <InputDate
        label="Дата выдачи"
        name="documentIssueDate"
        required
        tooltip="Какой-то текст"
        :incorrectFieldNotification="this.notifications__incorrect.documentIssueDate"
        @changed="collectStates" />
    </fieldset>

    <div class="submit-btn-wrapper">
      <input type="submit"
        class="submit-btn"
        :class="{ 'submit-btn_disabled': submitButtonDisabled }"
        :value="submitBtnValue" />
    </div>

  </form>
</template>

<script>
import InputText from './InputText.vue'
import InputDate from './InputDate.vue'
import InputPhoneNumber from './InputPhoneNumber.vue'
import CustomRadio from './CustomRadio.vue'
import InputSelectMultiple from './InputSelectMultiple.vue'
import InputSelect from './InputSelect.vue'
import InputCheckbox from './InputCheckbox.vue'

export default {
  name: 'TheForm',

  components: {
    InputText,
    InputDate,
    InputPhoneNumber,
    CustomRadio,
    InputSelectMultiple,
    InputSelect,
    InputCheckbox
  },
  data() {
    return {
      groupOptions: [{ title: "VIP", id: "abc" }, { title: "Проблемные", id: "bcd" }, { title: "ОМС", id: "cde" }],
      doctorOptions: [{ title: "Иванов", id: "def" }, { title: "Захаров", id: "efg" }, { title: "Чернышева", id: "fgh" }],
      documentTypeOptions: [{ title: "Паспорт", id: "ghi" }, { title: "Свидетельство о рождении", id: "hij" }, { title: "Вод. удостоверение", id: "ijk" }],
      
      requiredFields__correct: {
        lastname: undefined,
        firstname: undefined,
        phoneNumber: undefined,
        birthday: undefined,
        clientGroup: undefined,
        city: undefined,
        documentType: undefined,
        documentIssueDate: undefined,
      },
      notifications__incorrect: {
        lastname: undefined,
        firstname: undefined,
        phoneNumber: undefined,
        birthday: undefined,
        clientGroup: undefined,
        city: undefined,
        documentType: undefined,
        documentIssueDate: undefined,
      },
      submitButtonVisited: false,
    };
  },
  computed: {
    allRequiredFieldsCorrect() {
      let result = true;
      for (let fieldName in this.requiredFields__correct) {
        if (!this.requiredFields__correct[fieldName]) {
          result = false;
          break;
        }
      }
      return result;
    },
    submitButtonDisabled() {
      return this.submitButtonVisited && !this.allRequiredFieldsCorrect;
    },
    submitBtnValue() {
      return this.submitButtonDisabled ? 'Заполните обязательные поля' : 'Отправить';
    }
  },

  methods: {
    collectStates(field) {
      this.requiredFields__correct[field.name] = field.isCorrect;
    },
    
    verifyForm(event) {
      event.preventDefault();

      if (this.allRequiredFieldsCorrect) {
        event.target.submit();
      } else {
        this.submitButtonVisited = true;

        for (let fieldName in this.requiredFields__correct) {
          if (this.requiredFields__correct[fieldName]) {
            this.notifications__incorrect[fieldName] = false;
          } else {
            this.notifications__incorrect[fieldName] = true;
          }
        }
      }
    },
  }
}
</script>

<style>
form {
  max-width: 400px;
  margin: 40px auto 0;
  font-size: 1.25em;
}
fieldset {
  background-color: whitesmoke;
  border-radius: 0.5em;
  border: none;
  margin: 0 0 0.8em;
  padding: 0.5em 0.8em 0;
}
.fieldset-item {
  position: relative;
  margin-bottom: 0.4em;
}
.fieldset-item:last-child {
  margin-bottom: 0.8em;
}
.label_inline {
  cursor: pointer;
  display: inline-block;
  padding: 0.3em;
}


.incorrect-field-highlight {
  box-shadow: 0 0 0.1em 0px rgb(255, 102, 102);
  border-color: rgb(255, 102, 102);
}
.tooltip {
  font-size: 0.8em;
  position: absolute;
  bottom: 2.5em;
  right: 0;
}
.tooltip__body {
  box-shadow: 0 0 0.1em 0px rgb(255 102 102);
  background-color: rgb(255, 230, 230);
  border: 1px solid rgb(255, 102, 102);
  border-radius: 0.2em;
  display: block;
  line-height: 1;
  padding: 0.3em;
}
.tooltip__arrow {
  border-right: 0.4em solid transparent;
  border-left: 0.4em solid transparent;
  border-top: 0.4em solid rgb(255, 102, 102);
  float: right;
  margin-right: 0.75em;
}


.removed-checkbox {
  position: absolute;
  right: 0;
  top: 0;
  width: 0;
  height: 0;
  margin: 0;
  outline: none;
}

.submit-btn-wrapper {
  margin-bottom: 2em;
  padding: 0 0.8em;
  height: 2em;
}
.submit-btn {
  background-color: dimgrey;
  border: 1px solid dimgrey;
  border-radius: 0.2em;
  color: whitesmoke;
  cursor: pointer;
  font-size: inherit;
  width: 100%;
  height: 100%;
  padding: 0;
}
.submit-btn:active {
  transform: scale(99%);
}
.submit-btn_disabled {
  box-shadow: 0 0 0.3em rgb(255, 102, 102);
  background-color: rgb(255, 230, 230);
  border-color: rgb(255, 102, 102);
  color: inherit;
}
</style>