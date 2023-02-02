<template>
  <div class="profile-picture-section">
    <div class="avatar-parent-div">
      <img :src="avatar? avatar : require('../assets/images/avatar.png') " class="avatar"/>
      <div class="edit-avatar">
        <div class="buttons">
          <button>
            <label for="avatarImg">
              <span v-if="avatar">Change Avatar</span>
              <span v-else>Select Avatar</span>
            </label>
            <input type="file" ref="avatarImg" name="ChangeAvatar" id="avatarImg" @input="changeAvatar">
          </button>
          <button v-show="avatar" @click="removeAvatar">
            Remove Avatar
          </button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios'

export default {
  name: "ChangeAvatar",
  data() {
    return {
      localPersonalData: null,
      profileId: null,
      avatar: null
    }
  },
  methods: {
    changeAvatar() {
      if (this.$refs.avatarImg.files[0]) {
        this.avatar = URL.createObjectURL(this.$refs.avatarImg.files[0])
        this.axiosPut()
      }
      this.$emit('avatarChanged', this.avatar)
    },
    removeAvatar() {
      this.avatar = null
      this.axiosPut()
    },
    axiosPut() {
      this.localPersonalData.avatarImg = this.avatar
      axios
          .put('http://localhost:3000/profiles/' + this.profileId, this.localPersonalData)
          .then(() => {
            localStorage.setItem('personalInformation', JSON.stringify(this.localPersonalData))
          })
          .catch(err => console.log(err.message))

    }
  },
  mounted() {
    this.profileId = 1
    this.localPersonalData = JSON.parse(localStorage.getItem('personalInformation'))
    this.avatar = this.localPersonalData?.avatarImg
  }
}
</script>