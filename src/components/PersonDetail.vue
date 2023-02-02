<template>
  <div class="personal-data">
    <div class="name-section">
      <div>
        <h2 v-if="isEditFiled !=='name'">{{ fullName }}</h2>
        <div v-else>
          <div>
            <Form @submit="event.preventDefault()">
              <div class="validation-section">
                <label for="name">Name: </label>
                <Field name="name" type="name" id="name" v-model="localPersonalData.name" :rules="validateName"/>
              </div>
              <ErrorMessage name="name"/>
              <div class="validation-section">
                <label for="surname">Surname: </label>
                <Field name="surname" type="surname" id="surname" v-model="localPersonalData.surname"
                       :rules="validateName"/>
              </div>
              <ErrorMessage name="surname"/>
            </Form>
          </div>
          <div class="validation-section">
            <label for="middleName">Middle name: </label>
            <input type="text" id="middleName" v-model="localPersonalData.middleName"/>
          </div>
        </div>
      </div>
      <div class="edit-personal-data">
        <button v-show="isEditFiled !=='name'" class="edit-data-button" @click="editInfo('name')">
          <img :src="require('../assets/images/edit-data.png')">
        </button>
      </div>
    </div>
    <div class="birthdate-section">
      <div>
        <h4 v-if="isEditFiled !=='birthDate'">Birth date: {{ localPersonalData?.birthDate }}</h4>
        <div v-else>
          <label for="birthdate">Birth date: </label>
          <input type="date" id="birthdate" v-model="localPersonalData.birthDate"/>
        </div>
      </div>
      <div class="edit-personal-data">
        <button v-show="isEditFiled !=='birthDate'" class="edit-data-button" @click="editInfo('birthDate')">
          <img :src="require('../assets/images/edit-data.png')">
        </button>
      </div>
    </div>

    <div class="city-section">
      <div>
        <h4 v-if="isEditFiled !=='city'">City: {{ localPersonalData?.city }}</h4>
        <div v-else>
          <label for="city">City: </label>
          <input type="text" id="city" v-model="localPersonalData.city"/>
        </div>
      </div>
      <div class="edit-personal-data">
        <button v-show="isEditFiled !=='city'" class="edit-data-button" @click="editInfo('city')">
          <img :src="require('../assets/images/edit-data.png')">
        </button>
      </div>
    </div>
    <div class="email-section">
      <div>
        <h4 v-if="isEditFiled !=='email'">E-mail: {{ localPersonalData?.email }}</h4>
        <div v-else>
          <Form @submit="event.preventDefault()">
            <div class="validation-section">
              <label for="email">E-mail: </label>
              <Field name="email" type="email" id="email" v-model="localPersonalData.email" :rules="validateEmail"/>
            </div>
            <ErrorMessage name="email"/>
          </Form>
        </div>
      </div>
      <div class="edit-personal-data">
        <button v-show="isEditFiled !=='email'" class="edit-data-button" @click="editInfo('email')">
          <img :src="require('../assets/images/edit-data.png')">
        </button>
      </div>
    </div>
    <div class="phone-section">
      <div>
        <h4 v-if="isEditFiled !=='phone'">Telephone: {{ localPersonalData?.phone }}</h4>
        <div v-else>
          <label for="phone">Telephone: </label>
          <input type="tel" id="phone" v-model="localPersonalData.phone"/>
        </div>
      </div>
      <div class="edit-personal-data">
        <button v-show="isEditFiled !=='phone'" class="edit-data-button" @click="editInfo('phone')">
          <img :src="require('../assets/images/edit-data.png')">
        </button>
      </div>
    </div>
    <div class="languages-section">
      <h4>Languages: </h4>
      <div>
        <VueMultiselect
            v-model="localPersonalData.languages"
            :options="options"
            :multiple="true"
            :close-on-select="true"
            placeholder="Select languages"/>
      </div>
    </div>
    <div class="submit-button">
      <button @click="saveChanges">Save changes</button>
    </div>
  </div>
</template>

<script>
import {Form, Field, ErrorMessage} from 'vee-validate'
import VueMultiselect from 'vue-multiselect'
import axios from "axios"

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
    VueMultiselect
  },
  computed: {
    fullName() {
      return this.localPersonalData?.name + ' ' + this.localPersonalData?.surname + ' ' + this.localPersonalData?.middleName
    }
  },
  data() {
    return {
      localPersonalData: {},
      isEditFiled: null,
      options: ['English', 'Latin', 'Spain', 'Armenian', 'Russian', "German", "Chinese", 'Greek']
    }
  },
  methods: {
    editInfo(filed) {
      this.isEditFiled = filed;
    },
    validateEmail(value) {
      // if the field is empty
      if (!value) {
        return 'This field is required'
      }
      // if the field is not a valid email
      const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i
      if (!regex.test(value)) {
        return 'This field must be a valid email'
      }
      // All is good
      return true
    },
    validateName(value) {
      if (!value) {
        return 'This field is required'
      }
      if (/\d/.test(value)) {
        return 'Invalid name'
      }
      return true
    },
    saveChanges(){
      axios
          .put('http://localhost:3000/profiles/' + this.localPersonalData.id, this.localPersonalData)
          .then(()=> localStorage.setItem('personalInformation', JSON.stringify(this.localPersonalData)))
          .catch(err => console.log(err.message))
      this.isEditFiled = null
    }
  },
  mounted() {
    this.localPersonalData = JSON.parse(localStorage.getItem('personalInformation'))
  }
}
</script>

<style src="../assets/style/vue-multiselect-local.css"></style>