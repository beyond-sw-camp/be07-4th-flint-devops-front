<template>
  <v-dialog class="modal" v-model="localDialog" max-width="500px">
    <v-card>
      <v-card-title class="text-h5 text-center" style="font-family: Noto Serif KR; color: grey;">
        비밀번호 찾기
      </v-card-title>
      <v-card-text>
        <v-form @submit.prevent="findPassword">
          <v-text-field
            label="FIRST NAME(이름)"
            v-model="firstName"
            type="text"
            required
          ></v-text-field>
          <v-text-field
            label="LAST NAME(성)"
            v-model="lastName"
            type="text"
            required
          ></v-text-field>
          <v-text-field
            label="EMAIL"
            v-model="email"
            type="email"
            required
          ></v-text-field>
          <div style="display: flex; justify-content: center;">
              <v-btn type="submit" class="findButton" style="width: 150px; font-size: 15px; margin-right: 3px;">임시 비밀번호 발급</v-btn>
              <v-btn class="closeButton" @click="closeModal" style="width: 150px; font-size: 15px; margin-left: 3px;">닫기</v-btn>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from 'axios'

export default {
name: 'FindPasswordModal',
props: {
  modelValue: {
    type: Boolean,
    required: true
  },
  findPasswordEndpoint: {
    type: String,
    required: true
  }
},
data() {
  return {
    firstName: '',
    lastName: '',
    email: '',
    localDialog: this.modelValue
  }
},
watch: {
  modelValue(val) {
    this.localDialog = val
  },
  localDialog(val) {
    this.$emit('update:modelValue', val)
  }
},
methods: {
  async findPassword() {
    try {
      const response = await axios.post(this.findPasswordEndpoint, {
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email
      })
      alert(response.data.status_message)
      this.closeModal()
    } catch (e) {
      console.log(this.email)
      alert(e.response?.data?.error_message)
    }
  },
  closeModal() {
    this.localDialog = false
  }
}
}
</script>

<style scoped>
.moadl {
  font-family: "Noto Serif KR";
}
.findButton{
  background: grey;
  color: white;
  font-family: "Noto Serif KR";
  font-size: 14px;
}

.closeButton{
  background: white;
  color: grey;
  font-family: "Noto Serif KR";
  font-size: 14px;
}
</style>

