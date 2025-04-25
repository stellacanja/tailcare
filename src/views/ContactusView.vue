<script setup>
import { ref, watch } from 'vue'
import { useTheme } from 'vuetify'

import imgCatt from '@/assets/images/catt.gif'
import imgWel from '@/assets/images/welcome.png'
import imgMenu from '@/assets/images/menu.png'
import imgContact from '@/assets/images/contact.png'
import imgEmail from '@/assets/images/email.png'
import imgFb from '@/assets/images/fb.png'
import imgInsta from '@/assets/images/insta.png'

// Menu and dark mode state
const menu = ref(false)
const darkMode = ref(false)
const theme = useTheme()

// Define the color schemes for the navbar
const themes = {
  light: {
    '--navbar-bg': '#f4f5f7',
  },
  dark: {
    '--navbar-bg': '#210635',
  },
  custom: {
    '--navbar-bg': '#42od4b',
  },
}

// Watch for dark mode changes
watch(darkMode, (val) => {
  theme.global.name.value = val ? 'dark' : 'light'
})

// Change navbar color based on theme
function changeTheme() {
  theme.global.name.value = theme.global.name.value === 'light' ? 'dark' : 'light'

  const selectedTheme = themes[theme.global.name.value]
  Object.keys(selectedTheme).forEach((key) => {
    document.documentElement.style.setProperty(key, selectedTheme[key])
  })
}

const ex4 = ref([])

const symptoms = [
  'Lethargy',
  'Loss of appetite (anorexia)',
  'Vomiting',
  'Diarrhea',
  'Sneezing',
  'Coughing',
  'Nasal/eye discharge',
  'Fever',
  'Dehydration',
  'Weight loss',
  'Labored breathing',
  'Pale or yellow gums',
  'Excessive drooling',
  'Abdominal pain',
  'Hiding behavior',
  'Changes in urination',
]

function signOut() {
  alert('Signed out!')
}
</script>

<template>
  <v-app>
    <!-- App Bar -->
    <v-app-bar class="navbar" flat height="120" style="background-color: var(--navbar-bg)">
      <v-container class="d-flex justify-space-between align-center fill-height pa-0">
        <!-- Logo -->
        <div class="d-flex" style="line-height: 1; margin-top: 4px">
          <img :src="imgWel" alt="TailCare Logo" style="height: 100px; object-fit: contain" />
        </div>

        <!-- Menu with image icon -->
        <v-menu v-model="menu" offset-y>
          <template #activator="{ props }">
            <v-btn icon v-bind="props" class="mr-2">
              <img
                :src="imgMenu"
                alt="User Icon"
                style="width: 40px; height: 40px; object-fit: cover"
              />
            </v-btn>
          </template>
          <v-list>
            <!-- Change Theme -->
            <v-list-item @click="changeTheme">
              <v-list-item-title>Change Theme</v-list-item-title>
            </v-list-item>

            <!-- Contact Us as router link -->
            <v-list-item to="/contact" component="RouterLink">
              <v-list-item-title>Contact Us</v-list-item-title>
            </v-list-item>

            <!-- Sign Out -->
            <v-list-item @click="signOut">
              <v-list-item-title>Sign Out</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-container>
    </v-app-bar>

    <!-- Main Content -->
    <v-main>
      <v-container fluid class="pa-0">
        <!-- Hero Section with Background Image -->
        <div
          :style="{
            backgroundImage: `url(${imgContact})`,
            backgroundSize: 'cover',
            backgroundRepeat: 'no-repeat',
            backgroundPosition: 'center',
            minHeight: '600px',
            display: 'flex',
            flexDirection: 'column',
            justifyContent: 'center',
            alignItems: 'center',
            textAlign: 'center',
            color: 'purple',
            padding: '2rem',
          }"
        >
          <h1 style="font-size: 2.5rem; font-weight: bold">CONTACT US</h1>
          <p style="max-width: 600px; margin-top: 10px">
            Need an expert? You are more than welcomed to leave your contact info and we will be in
            touch shortly.
          </p>
        </div>

        <!-- Contact Info Cards -->
        <v-container class="mt-10">
          <v-row dense justify="center" align="center">
            <!-- Facebook Section -->
            <v-col cols="12" md="4" class="text-center">
              <v-card elevation="0">
                <v-card-text>
                  <v-img
                    :src="imgFb"
                    alt="Facebook"
                    max-width="48"
                    height="48"
                    class="mx-auto mb-2"
                  />
                  <h3 class="mt-4">FOLLOW US ON FACEBOOK</h3>
                  <p>Stay connected with us on Facebook for updates and more.</p>
                  <p style="color: purple; font-weight: bold">
                    <a
                      href="https://facebook.com"
                      target="_blank"
                      style="color: purple; text-decoration: none"
                    >
                      Visit our Facebook
                    </a>
                  </p>
                </v-card-text>
              </v-card>
            </v-col>

            <!-- Instagram Section -->
            <v-col cols="12" md="4" class="text-center">
              <v-card elevation="0">
                <v-card-text>
                  <v-img
                    :src="imgInsta"
                    alt="Instagram"
                    max-width="48"
                    height="48"
                    class="mx-auto mb-2"
                  />
                  <h3 class="mt-4">FOLLOW US ON INSTAGRAM</h3>
                  <p>Catch up with us on Instagram for the latest trends and posts.</p>
                  <p style="color: purple; font-weight: bold">
                    <a
                      href="https://instagram.com"
                      target="_blank"
                      style="color: purple; text-decoration: none"
                    >
                      Visit our Instagram
                    </a>
                  </p>
                </v-card-text>
              </v-card>
            </v-col>

            <!-- Email Section -->
            <v-col cols="12" md="4" class="text-center">
              <v-card elevation="0">
                <v-card-text>
                  <v-img
                    :src="imgEmail"
                    alt="Logo"
                    max-width="48"
                    height="48"
                    class="mx-auto mb-2"
                  />
                  <h3 class="mt-4">CONTACT US</h3>
                  <p>Send us an email and we'll respond promptly.</p>
                  <p style="color: purple; font-weight: bold">money@myemail.com</p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-container>
    </v-main>

    <!-- Footer -->
    <v-footer border app class="justify-center" style="background-color: #f4f5f7">
      TailCare@2025
    </v-footer>
  </v-app>
</template>

<style scoped>
/* Default light theme navbar */
:root {
  --navbar-bg: #f4f5f7;
}

/* Dark theme navbar */
[data-theme='dark'] {
  --navbar-bg: #210635;
}

/* Custom theme navbar */
[data-theme='custom'] {
  --navbar-bg: #42od4b;
}
</style>
