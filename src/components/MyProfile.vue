<template>
  <div class="container">
    <div class="welcome-section">
      <div class="left-side">
        <div class="welcome-image-section">
          <img :src="personalInformation?.avatarImg? personalInformation?.avatarImg : require('../assets/images/avatar.png') " class="avatar"/>
        </div>
        <div class="welcome-name-section">
          <h3>{{ personalInformation?.name + ' ' + personalInformation?.surname }}</h3>
        </div>
        <div class="welcome-select-section">
          <button @click="selectSection('changeAvatar')"
                  :class="{'active-section' : selectedSection === 'changeAvatar'}">Change Avatar
          </button>
          <button @click="selectSection('personalInfo')"
                  :class="{'active-section' : selectedSection === 'personalInfo'}">Personal information
          </button>
          <button @click="selectSection('password')" :class="{'active-section' : selectedSection === 'password'}">
            Password
          </button>
        </div>
      </div>

      <div class="right-side">
        <change-avatar v-if="selectedSection==='changeAvatar'" @avatarChanged="avatarChanged"/>
        <person-detail v-if="selectedSection==='personalInfo'"/>
        <change-password v-if="selectedSection==='password'"/>
      </div>
    </div>
  </div>

</template>

<script>
import ChangeAvatar from "@/components/ChangeAvatar"
import ChangePassword from "@/components/ChangePassword"
import PersonDetail from "@/components/PersonDetail"

import axios from "axios"

export default {
  name: "MyProfile",
  components: {ChangeAvatar, ChangePassword, PersonDetail},
  data() {
    return {
      profileId: null,
      selectedSection: 'personalInfo',
      personalInformation: null
    }
  },
  methods: {
    selectSection(section) {
      this.selectedSection = section
    },
    avatarChanged(avatar) {
      this.personalInformation.avatarImg = avatar
    }
  },
  mounted() {
    this.profileId = 1;
    axios
        .get('http://localhost:3000/profiles/' + this.profileId)
        .then(response => this.personalInformation = response.data)
        .then(() => localStorage.setItem('personalInformation', JSON.stringify(this.personalInformation)))

  }


}
</script>

<style scoped>

</style>