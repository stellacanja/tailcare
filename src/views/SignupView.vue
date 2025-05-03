<script setup>
import {
  requiredValidator,
  emailValidator,
  passwordValidator,
  confirmedValidator
} from '@/utils/validators'
import { ref } from 'vue'
import imgSix from '@/assets/images/six.png'
import imgWel from '@/assets/images/welcome.png'

const username = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')

const formDataDefault = {
  username: '',
  email: '',
  password:'',
  confirmPassword:''
}

const formData = ref({
  ...formDataDefault
})

const refVForm = ref()

const onSubmit = () => {
  alert(formData.value.password)
}

const onFormSubmit = () => {
  refVForm.value?.validate().then(({ valid }) => {
    if (valid) onSubmit()
  })
}
</script>

<template>
  <v-responsive class="">
    <v-app>
      <!-- Centered App Bar Logo -->
      <v-app-bar class="navbar" flat height="150" style="background-color: #f4f5f7">
        <v-container class="d-flex justify-center align-center fill-height pa-0">
          <div class="d-flex flex-column align-center" style="line-height: 1; margin-top: 4px">
            <img :src="imgWel" alt="TailCare Logo" style="height: 150px; object-fit: contain" />
          </div>
        </v-container>
      </v-app-bar>

      <v-main>
        <v-container>
          <v-row class="align-center">
            <!-- Registration Form -->
            <v-col cols="12" md="5" class="mt-8">
              <v-card class="pa-4">
                <h1 id="welc" style="color: #8c52ff" class="d-flex justify-center">
                  <b>REGISTER NOW</b>
                </h1>
                <p class="d-flex justify-center" style="margin: 2%; color: skyblue">
                  Create account to get started
                </p>
                <v-form ref="refVForm" @submit.prevent="onFormSubmit">
                  <v-text-field
                    v-model="formData.username"
                    variant="solo-inverted"
                    label="Username"
                    type="text"
                    :rules="[requiredValidator]"
                  />
                  <v-text-field
                    v-model="formData.email"
                    variant="solo-inverted"
                    label="Email address"
                    type="email"
                    :rules="[requiredValidator, emailValidator]"
                  />
                  <v-text-field
                    v-model="formData.password"
                    variant="solo-inverted"
                    label="Create Password"
                    type="password"
                    :rules="[requiredValidator, passwordValidator]"
                  />
                  <v-text-field
                    v-model="formData.confirmPassword"
                    variant="solo-inverted"
                    label="Confirm Password"
                    type="password"
                    :rules="[requiredValidator, confirmedValidator(formData.confirmPassword, formData.password)]"
                  />
                  <v-btn class="mt-2" type="submit" style="background-color: skyblue" block>
                    Sign Up
                  </v-btn>
                </v-form>
              </v-card>

              <div class="mt-4">
                <h4 style="color: #8c52ff; font-style: italic">
                  Already have an existing account?
                  <RouterLink to="/" style="color: skyblue; font-style: normal">Log In</RouterLink>
                </h4>
              </div>
            </v-col>

            <!-- Image Section -->
            <v-col cols="12" md="7" class="d-flex justify-center mt-6 mt-md-0">
              <img
                :src="imgSix"
                alt="Pets"
                style="max-width: 100%; height: auto; object-fit: cover; border-radius: 8px"
              />
            </v-col>
          </v-row>
        </v-container>
      </v-main>

      <v-footer border app id="foot" class="justify-center" style="background-color: #f4f5f7">
        TailCare@2025
      </v-footer>
    </v-app>
  </v-responsive>
</template>
