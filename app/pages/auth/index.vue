<template>
  <v-container>
    <v-layout>
      <v-flex xs10 offset-xs1 sm6 offset-sm3>
        <v-alert
          :value="isSuccess"
          color="success"
          icon="check_circle"
          transition="scale-transition"
          outline
        >
          This is a success alert.
        </v-alert>
        <v-alert
          :value="errors.isError"
          color="error"
          icon="warning"
          transition="scale-transition"
          outline
          v-for="msg in errors.full_messages"
          :key="msg"
        >
          {{ msg }}
        </v-alert>
        <v-text-field
          v-model="formData.name"
          type="text"
          label="Name"
          style="margin-top: 20px;"
        ></v-text-field>
        <v-text-field
          v-model="formData.email"
          type="text"
          label="Email"
        ></v-text-field>
        <v-text-field
          v-model="formData.password"
          type="password"
          label="Password"
        ></v-text-field>
        <div class="text-sm-center">
          <v-btn round @click="handleClickSubmit" dark>submit</v-btn>
        </div>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import { mapActions } from 'vuex'
export default {
  asyncData({ redirect, store }) {
    return {
      formData: {
        name: '',
        email: '',
        password: ''
      },
      isSuccess: false,
      errors: {
        isError: false,
        full_messages: []
      }
    }
  },
  methods: {
    async handleClickSubmit() {
      try {
        await this.signUp({ ...this.formData })
        this.$data.formData.name = ''
        this.$data.formData.email = ''
        this.$data.formData.password = ''
        this.$data.errors.isError = false
        this.$data.isSuccess = true
        await new Promise(resolve => setTimeout(resolve, 2000))
        this.$data.isSuccess = false
      } catch (e) {
        this.$data.errors.isError = true
        this.$data.errors.full_messages = e
      }
    },
    ...mapActions(['signUp'])
  }
}
</script>
