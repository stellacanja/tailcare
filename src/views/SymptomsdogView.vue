<script setup>
import { ref, watch } from 'vue'
import { useTheme } from 'vuetify'
import { useRouter } from 'vue-router'

import imgType from '@/assets/images/type.gif'
import imgWel from '@/assets/images/welcome.png'
import imgMenu from '@/assets/images/menu.png'

// Setup
const menu = ref(false)
const darkMode = ref(false)
const theme = useTheme()
const router = useRouter()

// Navbar themes
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

// Theme watcher
watch(darkMode, (val) => {
  theme.global.name.value = val ? 'dark' : 'light'
})

// Change theme
function changeTheme() {
  theme.global.name.value = theme.global.name.value === 'light' ? 'dark' : 'light'
  const selectedTheme = themes[theme.global.name.value]
  Object.keys(selectedTheme).forEach((key) => {
    document.documentElement.style.setProperty(key, selectedTheme[key])
  })
}

// Symptoms list
const ex4 = ref([])

const symptoms = [
  'Lethargy (low energy)',
  'Loss of appetite',
  'Vomiting',
  'Diarrhea',
  'Coughing',
  'Sneezing',
  'Nasal discharge',
  'Fever',
  'Weight loss',
  'Difficulty breathing',
  'Dehydration',
  'Weakness',
  'Abdominal pain',
  'Excessive drooling',
  'Pale gums',
  'Shivering or trembling',
]

// Sign out function
function signOut() {
  alert('Signed out!')
}

// Contact Us: show alert then navigate
function contactUs() {
  alert('Contact us at support@tailcare.com!')
  setTimeout(() => {
    router.push('/contact')
  }, 500) // delay for smooth UX
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

        <!-- Menu with icon -->
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
            <v-list-item @click="changeTheme">
              <v-list-item-title>Change Theme</v-list-item-title>
            </v-list-item>

            <v-list-item @click="contactUs">
              <v-list-item-title>Contact Us</v-list-item-title>
            </v-list-item>

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
          <!-- Symptoms List -->
          <v-col cols="12" md="5">
            <v-card class="pa-6" style="height: 500px; overflow-y: auto">
              <v-row>
                <v-col cols="12" v-for="(symptom, i) in symptoms" :key="i">
                  <v-checkbox v-model="ex4" :label="symptom" :value="symptom" color="warning" />
                </v-col>
              </v-row>
            </v-card>
          </v-col>

          <!-- Image -->
          <v-col cols="12" md="7" class="d-flex justify-center align-center">
            <img :src="imgType" alt="Four Pets" style="height: 500px; width: 700px" />
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
:root {
  --navbar-bg: #f4f5f7;
}
[data-theme='dark'] {
  --navbar-bg: #210635;
}
[data-theme='custom'] {
  --navbar-bg: #42od4b;
}
</style>
