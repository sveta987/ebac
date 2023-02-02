<template>

  <div class="change-password">
    <Form @submit="event.preventDefault()">
      <div v-if="!isPasswordTrue" class="validation-section">
        <label for="currentPassword">Current password:  </label>
        <Field name="currentPassword" type="password" id="currentPassword" :rules="validateCurrentPassword"/>
      </div>
      <ErrorMessage name="currentPassword"/>
      <div v-if="isPasswordTrue">
        <div class="validation-section">
          <label for="newPassword">'New password: ' </label>
          <Field name="newPassword" type="password" id="newPassword" v-model="newPassword"
                 :rules="validateNewPassword"/>
        </div>
        <ErrorMessage name="newPassword"/>

        <div class="validation-section">
          <label for="confirmPassword">'Confirm password: ' </label>
          <Field name="confirmPassword" type="password" id="confirmPassword" :rules="validatePassword"/>
        </div>
        <ErrorMessage name="confirmPassword"/>
        <button class="submit-button" @click="savePassword">Save new password</button>
      </div>

    </Form>

  </div>
</template>

<script>
import {Form, Field, ErrorMessage} from 'vee-validate'
import VueMultiselect from "vue-multiselect"

export default {
  name: "ChangePassword",
  components: {
    Form,
    Field,
    ErrorMessage,
    VueMultiselect
  },
  data() {
    return {
      isPasswordTrue: false,
      localPersonalData: null,
      newPassword: null,
    }
  },
  methods: {
    validateCurrentPassword(value) {
      if (!value) {
        return 'This field is required'
      }
      if (value !== this.localPersonalData.password) {
        return 'Incorrect password'
      }
      this.isPasswordTrue = true
      return true
    },
    validateNewPassword(value) {
      if (!value) {
        return 'This field is required'
      }
      if (value.length < 8) {
        return 'Password must contain at least 8 characters'
      }
      this.newPassword = value
      return true
    },
    validatePassword(value) {
      if (!value) {
        return 'This field is required'
      }
      if (value !== this.newPassword) {
        return 'Passwords do not match'
      }
      this.localPersonalData.password = this.newPassword
      return true
    },
    savePassword(){
      this.isPasswordTrue = false
    }
  },
  mounted() {
    this.localPersonalData = JSON.parse(localStorage.getItem('personalInformation'))
  }
}
</script>