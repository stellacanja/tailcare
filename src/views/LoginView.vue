<script setup>
import { requiredValidator, emailValidator } from '@/utils/validators'
import { ref } from 'vue'
import imgFour from '@/assets/images/four.png'
import imgWel from '@/assets/images/welcome.png'

const email = ref('')
const password = ref('')
const refVForm = ref()

const formDataDefault = {
  email: '',
  password: ''
}

const formData= ref({
  ...formDataDefault
})

const onSubmit = () => {
  //alert(formData.value.password)
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
            <!-- Login Form -->
            <v-col cols="12" md="5">
              <v-card class="pa-4 mt-8">
                <h1 id="welc" style="color: #8c52ff" class="d-flex justify-center">
                  <b>WELCOME OWNERS</b>
                </h1>
                <v-form ref="refVForm" @submit.prevent="onFormSubmit">
                  <v-text-field
                  v-model="formData.email"
                    label="Email address"
                    type="email"
                    variant="solo-inverted"
                    :rules="[requiredValidator, emailValidator]"
                  />
                  <v-text-field
                  v-model="formData.password"
                    label="Password"
                    type="password"
                    variant="solo-inverted"
                    :rules="[requiredValidator]"
                  />
                  <v-btn
                    variant="tonal"
                    class="mt-2"
                    type="submit"
                    style="background-color: skyblue"
                    block
                  >
                    Log in
                  </v-btn>
                </v-form>
              </v-card>

              <div class="mt-4">
                <h4 style="font-style: italic; color: #8c52ff">
                  Don't have an account?
                  <RouterLink to="/sign" style="color: skyblue; font-style: normal">
                    Sign Up
                  </RouterLink>
                </h4>
              </div>
              <div class="mt-2">
                <h4>
                  <RouterLink
                    to=""
                    style="color: skyblue; font-style: normal"
                    class="d-flex justify-end"
                  >
                    Forgot your password?
                  </RouterLink>
                </h4>
              </div>
            </v-col>

            <!-- Image Column -->
            <v-col cols="12" md="7" class="d-flex justify-center mt-6 mt-md-0">
              <img
                :src="imgFour"
                alt="Four Pets"
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
