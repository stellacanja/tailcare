<script setup>
import { ref } from 'vue'
import imgSix from '@/assets/images/six.png'
import imgWel from '@/assets/images/welcome.png'
import imgEmail from '@/assets/images/email.png'
import imgFb from '@/assets/images/fb.png'
import imgInsta from '@/assets/images/insta.png'

const username = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')

// Password visibility toggles
const showPassword = ref(false)
const showConfirmPassword = ref(false)
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
        <v-container fluid>
          <v-row no-gutters>
            <!-- Left Image Section -->
            <v-col
              cols="12"
              md="6"
              class="pa-6 d-flex align-center justify-center"
              style="background-color: #f4f5f7"
            >
              <img
                :src="imgSix"
                alt="Illustration"
                style="width: 100%; max-width: 600px; height: auto"
              />
            </v-col>

            <!-- Right Form Section -->
            <v-col cols="12" md="6" class="pa-6">
              <div class="pa-6">
                <h1 class="text-h4 font-weight-bold mb-2" style="color: #8c52ff">REGISTER NOW</h1>
                <p class="mb-6" style="color: skyblue">Create account to get started</p>

                <v-form fast-fail @submit.prevent>
                  <v-text-field
                    v-model="username"
                    variant="solo-inverted"
                    label="Username"
                    prepend-inner-icon="mdi-account"
                    type="text"
                  />
                  <v-text-field
                    v-model="email"
                    variant="solo-inverted"
                    label="Email address"
                    prepend-inner-icon="mdi-email"
                    type="email"
                  />
                  <v-text-field
                    v-model="password"
                    :type="showPassword ? 'text' : 'password'"
                    variant="solo-inverted"
                    label="Create Password"
                    prepend-inner-icon="mdi-lock"
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
                  <v-text-field
                    v-model="confirmPassword"
                    :type="showConfirmPassword ? 'text' : 'password'"
                    variant="solo-inverted"
                    label="Confirm Password"
                    prepend-inner-icon="mdi-lock-check"
                  >
                    <template #append-inner>
                      <v-fade-transition>
                        <v-icon
                          :key="showConfirmPassword"
                          class="mr-2"
                          @click="showConfirmPassword = !showConfirmPassword"
                          style="cursor: pointer"
                        >
                          {{ showConfirmPassword ? 'mdi-eye-off' : 'mdi-eye' }}
                        </v-icon>
                      </v-fade-transition>
                    </template>
                  </v-text-field>
                  <v-btn class="mt-4" style="background-color: skyblue" type="submit" block>
                    Sign Up
                  </v-btn>
                </v-form>

                <!-- Social login section -->
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
                  <span>Already have an account?</span>
                  <RouterLink to="/" style="color: skyblue"> Log In</RouterLink>
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
