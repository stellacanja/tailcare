<script setup>
import { ref, watch } from 'vue'
import { useTheme } from 'vuetify'

import imgCatt from '@/assets/images/catt.gif'
import imgWel from '@/assets/images/welcome.png'
import imgMenu from '@/assets/images/menu.png'

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

// Reactive model to store selected symptoms
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
      <v-container class="mt-10">
        <v-row>
          <!-- Checkboxes Section -->
          <v-col cols="12" md="5">
            <v-card class="pa-6" style="height: 500px; overflow-y: auto">
              <v-row>
                <v-col cols="12" v-for="(symptom, i) in symptoms" :key="i">
                  <v-checkbox v-model="ex4" :label="symptom" :value="symptom" color="warning" />
                </v-col>
              </v-row>
            </v-card>
          </v-col>

          <!-- Image Section -->
          <v-col cols="12" md="7" class="d-flex justify-center align-center">
            <img :src="imgCatt" alt="Four Pets" style="height: 500px; width: auto" />
          </v-col>
        </v-row>
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
