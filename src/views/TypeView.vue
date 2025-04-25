<script setup>
import { ref, watch } from 'vue'
import { useTheme } from 'vuetify'

import imgWel from '@/assets/images/welcome.png'
import imgCat from '@/assets/images/cat.png'
import imgDog from '@/assets/images/dog.png'
import imgMenu from '@/assets/images/menu.png'

const menu = ref(false)
const darkMode = ref(false)
const theme = useTheme()

// Define the color schemes for the navbar only
const themes = {
  light: {
    '--navbar-bg': '#f4f5f7', // Light background for navbar
  },
  dark: {
    '--navbar-bg': '#210635', // Dark Purple background for navbar
  },
  custom: {
    '--navbar-bg': '#42od4b', // Custom Green background for navbar
  },
}

// Watch for theme changes
watch(darkMode, (val) => {
  theme.global.name.value = val ? 'dark' : 'light'
})

function changeTheme() {
  // Switch between light, dark, and custom themes
  theme.global.name.value = theme.global.name.value === 'light' ? 'dark' : 'light'

  // Apply the selected theme's navbar color
  const selectedTheme = themes[theme.global.name.value]
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
      <!-- App Bar (thinner height) -->
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

              <!-- Contact Us Option as a Router Link -->
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
        <v-container class="mt-10">
          <v-row>
            <!-- Centered Heading -->
            <v-col
              cols="12"
              class="text-center mb-4"
              style="
                font-size: 1.5rem;
                font-weight: bold;
                color: #6667ab;
                letter-spacing: 1px;
                text-transform: uppercase;
              "
            >
              <h1>Please choose your guardian's type</h1>
            </v-col>

            <!-- Images -->
            <v-col cols="12" md="6" class="d-flex justify-center">
              <RouterLink to="/symptomscat">
                <img :src="imgCat" alt="Cat" style="height: 500px; width: auto" />
              </RouterLink>
            </v-col>
            <v-col cols="12" md="6" class="d-flex justify-center">
              <RouterLink to="/symptomsdog">
                <img :src="imgDog" alt="Dog" style="height: 500px; width: auto" />
              </RouterLink>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-app>
  </v-responsive>
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
