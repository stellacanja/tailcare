<script setup>
import { ref } from 'vue'
import imgDog from '@/assets/images/doggo.gif'
import imgWel from '@/assets/images/welcome.png'
import imgMenu from '@/assets/images/menu.png'

const menu = ref(false)
const theme = ref('light')

// Define the color schemes for the navbar only
const themes = {
  light: {
    '--navbar-bg': '#f5d5e0', // Soft Pink
  },
  dark: {
    '--navbar-bg': '#210635', // Dark Purple
  },
  custom: {
    '--navbar-bg': '#42od4b', // Green
  },
}

function changeTheme() {
  // Switch between light, dark, and custom themes
  theme.value = theme.value === 'light' ? 'dark' : theme.value === 'dark' ? 'custom' : 'light'

  // Apply the selected theme's navbar color
  const selectedTheme = themes[theme.value]
  Object.keys(selectedTheme).forEach((key) => {
    document.documentElement.style.setProperty(key, selectedTheme[key])
  })
}

function signOut() {
  alert('Signed out!')
}
</script>

<template>
  <v-responsive>
    <v-app>
      <!-- App Bar (Navbar) -->
      <v-app-bar class="navbar" flat height="80" style="background-color: var(--navbar-bg)">
        <v-container class="d-flex justify-space-between align-center fill-height pa-0">
          <!-- Logo -->
          <div class="d-flex" style="line-height: 1; margin-top: 4px">
            <img :src="imgWel" alt="TailCare Logo" style="height: 80px; object-fit: contain" />
          </div>

          <!-- Menu with image icon -->
          <v-menu v-model="menu" offset-y>
            <template #activator="{ props }">
              <v-btn icon v-bind="props" class="mr-2">
                <img
                  :src="imgMenu"
                  alt="Menu Icon"
                  style="width: 40px; height: 40px; object-fit: cover"
                />
              </v-btn>
            </template>

            <v-list>
              <!-- Change Theme Button -->
              <v-list-item @click="changeTheme">
                <v-list-item-title>Change Theme</v-list-item-title>
              </v-list-item>

              <!-- Contact Us Router Link -->
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
        <v-container class="d-flex justify-center">
          <div class="d-flex flex-column align-center">
            <img :src="imgDog" alt="doggo" style="height: 500px; width: 500px" />
            <v-btn
              to="/type"
              variant="text"
              class="mt-0"
              style="color: #7b337e; font-style: inherit"
            >
              Tap to continue
            </v-btn>
          </div>
        </v-container>
      </v-main>
    </v-app>
  </v-responsive>
</template>

<style scoped>
/* Default light theme navbar */
:root {
  --navbar-bg: #f5d5e0;
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
