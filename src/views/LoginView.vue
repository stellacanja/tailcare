<script setup>
import { requiredValidator, emailValidator } from '@/utils/validators'
import { ref } from 'vue'
import imgFour from '@/assets/images/four.png'
import imgWel from '@/assets/images/welcome.png'
import imgEmail from '@/assets/images/email.png'
import imgFb from '@/assets/images/fb.png'
import imgInsta from '@/assets/images/insta.png'

const email = ref('')
const password = ref('')
<<<<<<< HEAD
const showPassword = ref(false)

const rules = {
  required: (value) => !!value || 'Required.',
  min: (v) => v.length >= 8 || 'Min 8 characters',
=======
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
>>>>>>> bff16da8d674d7ffac8a1c37cb54c6293ff8b8da
}
</script>

<template>
  <v-responsive>
    <v-app>
      <!-- Top Logo -->
      <v-app-bar flat height="150" style="background-color: #f4f5f7">
        <v-container class="d-flex justify-center align-center fill-height pa-0">
          <img
            :src="imgWel"
            alt="TailCare Logo"
            style="height: 100%; max-height: 130px; object-fit: contain"
          />
        </v-container>
      </v-app-bar>

      <!-- Main Content -->
      <v-main>
<<<<<<< HEAD
        <v-container fluid>
          <v-row no-gutters>
            <!-- Left Image Section -->
            <v-col
              cols="12"
              md="6"
              class="pa-6 d-flex align-center justify-center"
              style="background-color: #f4f5f7"
            >
              <img :src="imgFour" alt="Pets" style="width: 100%; max-width: 600px; height: auto" />
            </v-col>

            <!-- Right Form Section -->
            <v-col cols="12" md="6" class="pa-6">
              <div class="pa-6">
                <h1 class="text-h4 font-weight-bold mb-2" style="color: #8c52ff">WELCOME OWNERS</h1>
                <p class="mb-6" style="color: skyblue">Log in to your account</p>

                <v-form fast-fail @submit.prevent>
                  <!-- Email field -->
                  <v-text-field
                    v-model="email"
                    variant="solo-inverted"
                    label="Email address"
                    type="email"
                    prepend-inner-icon="mdi-email"
                    :rules="[rules.required]"
=======
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
>>>>>>> bff16da8d674d7ffac8a1c37cb54c6293ff8b8da
                  />

                  <!-- Password field with animated eye icon -->
                  <v-text-field
<<<<<<< HEAD
                    v-model="password"
                    :type="showPassword ? 'text' : 'password'"
                    variant="solo-inverted"
                    label="Password"
                    prepend-inner-icon="mdi-lock"
                    :rules="[rules.required, rules.min]"
                  >
                    <template #append-inner>
                      <v-fade-transition>
                        <v-icon
                          :key="showPassword"
                          class="mr-2"
                          @click="showPassword = !showPassword"
                          style="cursor: pointer"
                        >
                          {{ showPassword ? 'mdi-eye-off' : 'mdi-eye' }}
                        </v-icon>
                      </v-fade-transition>
                    </template>
                  </v-text-field>

=======
                  v-model="formData.password"
                    label="Password"
                    type="password"
                    variant="solo-inverted"
                    :rules="[requiredValidator]"
                  />
>>>>>>> bff16da8d674d7ffac8a1c37cb54c6293ff8b8da
                  <v-btn
                    class="mt-4"
                    style="background-color: skyblue"
                    type="submit"
                    block
                    to="/doggo"
                  >
                    Log in
                  </v-btn>
                </v-form>

                <!-- Forgot Password -->
                <div class="d-flex justify-end mt-2">
                  <RouterLink to="/" style="color: skyblue">Forgot your password?</RouterLink>
                </div>

                <!-- Social login -->
                <div class="text-center mt-6 mb-4">
                  <p>Or continue with</p>
                  <div class="d-flex justify-center flex-wrap gap-4 mt-2">
                    <a
                      href="https://web.facebook.com/people/Tailcare-Tailcare/pfbid0cMKJ9HjWU3BgtcnEBTaPBoWfTa6gizYes15NZVPwqk4k1f88AV8g1xbh46d4V3XVl/"
                      target="_blank"
                    >
                      <v-btn icon>
                        <v-img :src="imgFb" alt="Facebook" width="24" height="24" />
                      </v-btn>
                    </a>
                    <a
                      href="https://www.instagram.com/tailcare_2025?igsh=MWJzdTFzdHd3cHFvaQ%3D%3D"
                      target="_blank"
                    >
                      <v-btn icon>
                        <v-img :src="imgInsta" alt="Instagram" width="24" height="24" />
                      </v-btn>
                    </a>
                    <a href="mailto:tailcare2025@gmail.com">
                      <v-btn icon>
                        <v-img :src="imgEmail" alt="Email" width="24" height="24" />
                      </v-btn>
                    </a>
                  </div>
                </div>

                <div class="text-center">
                  <span>Don't have an account?</span>
                  <RouterLink to="/sign" style="color: skyblue"> Sign Up</RouterLink>
                </div>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-main>

      <!-- Footer -->
      <v-footer app class="justify-center" style="background-color: #f4f5f7">
        <div class="text-center" style="color: #8c52ff">
          <p>TailCare@2025</p>
        </div>
      </v-footer>
    </v-app>
  </v-responsive>
</template>

<style scoped>
.text-center p {
  margin-bottom: 10px;
}
</style>
