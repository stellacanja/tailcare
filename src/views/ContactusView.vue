<script setup>
import { ref, watchEffect } from 'vue'
import imgWel from '@/assets/images/welcome.png'
import imgCatt from '@/assets/images/catt.gif'
import imgContact from '@/assets/images/contact.png'
import imgFb from '@/assets/images/fb.png'
import imgInsta from '@/assets/images/insta.png'
import imgEmail from '@/assets/images/email.png'

// Theme setup
const theme = ref('light')
const consultOpen = ref(false)
const typeOpen = ref(false)
const currentTime = ref(new Date().toLocaleString())

const themes = {
  light: { '--second-column-bg': '#f5d5e0' },
  dark: { '--second-column-bg': '#210635' },
  custom: { '--second-column-bg': '#42od4b' },
}

function changeTheme() {
  theme.value = theme.value === 'light' ? 'dark' : theme.value === 'dark' ? 'custom' : 'light'
}

watchEffect(() => {
  const selectedTheme = themes[theme.value]
  Object.keys(selectedTheme).forEach((key) => {
    document.documentElement.style.setProperty(key, selectedTheme[key])
  })
})

function signOut() {
  alert('Signed out!')
}

function toggleConsult() {
  consultOpen.value = !consultOpen.value
}

function toggleType() {
  typeOpen.value = !typeOpen.value
}

function updateTime() {
  setInterval(() => {
    currentTime.value = new Date().toLocaleString()
  }, 1000)
}
updateTime()

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
</script>

<template>
  <v-app>
    <v-main>
      <!-- Sidebar -->
      <v-navigation-drawer app permanent width="250">
        <v-container class="text-center mt-4">
          <img :src="imgWel" alt="Welcome Icon" style="height: 100px; width: auto" />
          <h1 class="text-h5 font-weight-bold custom-title">Welcome Owner</h1>
        </v-container>

        <v-list dense>
          <v-list-item to="/doggo" component="RouterLink" class="menu-item">
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item @click="toggleConsult" class="menu-item">
            <v-list-item-title>Consult</v-list-item-title>
          </v-list-item>

          <v-list-item v-if="consultOpen" @click="toggleType" class="menu-item">
            <v-list-item-title>Type</v-list-item-title>
          </v-list-item>

          <v-list-item v-if="typeOpen" to="/symptomscat" component="RouterLink" class="menu-item">
            <v-list-item-title>Cat</v-list-item-title>
          </v-list-item>
          <v-list-item v-if="typeOpen" to="/symptomsdog" component="RouterLink" class="menu-item">
            <v-list-item-title>Dog</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item to="/contact" component="RouterLink" class="menu-item">
            <v-list-item-title>Contact Us</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item @click="changeTheme" class="menu-item">
            <v-list-item-title>Change Theme</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item @click="signOut" class="menu-item">
            <v-list-item-title>Sign Out</v-list-item-title>
          </v-list-item>

          <v-card class="date-time-card mt-5" style="padding: 16px; text-align: center">
            <v-card-title class="text-h6">Current Date and Time</v-card-title>
            <v-card-subtitle>
              <div>{{ currentTime }}</div>
            </v-card-subtitle>
          </v-card>
        </v-list>
      </v-navigation-drawer>

      <!-- Main Section with Contact Us Content -->
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
                      href="https://www.facebook.com/profile.php?id=61575211577548"
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
                      href="https://www.instagram.com/tailcare_2025?igsh=MWJzdTFzdHd3cHFvaQ=="
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
                    alt="Email"
                    max-width="48"
                    height="48"
                    class="mx-auto mb-2"
                  />
                  <h3 class="mt-4">CONTACT US</h3>
                  <p>Send us an email and we'll respond promptly.</p>
                  <p style="color: purple; font-weight: bold">tailcare2025@gmail.com</p>
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
:root {
  --second-column-bg: #f5d5e0;
}

[data-theme='dark'] {
  --second-column-bg: #210635;
}

[data-theme='custom'] {
  --second-column-bg: #42od4b;
}

.custom-title {
  font-family: 'Lora', serif;
  color: #7b466a;
  font-weight: bold;
  margin-bottom: 10px;
}

.menu-item {
  padding: 16px 20px;
  margin-bottom: 12px;
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  color: #7b466a;
}

.menu-item:hover {
  background-color: #efefef;
}

body {
  font-family: 'Roboto', sans-serif;
}

.date-time-card {
  background-color: #d391b0;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  height: 180px;
  margin-top: 16px;
}

.date-time-card .text-h6,
.date-time-card .v-card-subtitle {
  color: #5d3c64;
  font-family: 'Roboto', sans-serif;
  font-weight: bold;
  font-size: 18px;
}

.second-column-background {
  background-color: var(--second-column-bg);
}
</style>

<link
  href="https://fonts.googleapis.com/css2?family=Lora:wght@700&family=Roboto:wght@400&display=swap"
  rel="stylesheet"
/>
