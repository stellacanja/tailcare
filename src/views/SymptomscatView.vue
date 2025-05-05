<script setup>
import { ref, watch, watchEffect } from 'vue'
import imgWel from '@/assets/images/welcome.png'
import imgCatt from '@/assets/images/catt.gif'

// Sidebar toggle
const drawer = ref(true)

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
const possibleIllnesses = ref([])

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

const symptomIllnessMap = {
  Lethargy: ['Anemia', 'Infection'],
  'Loss of appetite (anorexia)': ['Gastroenteritis', 'Kidney Disease'],
  Vomiting: ['Food Allergy', 'Parvovirus'],
  Diarrhea: ['Food Intolerance', 'Parasites'],
  Sneezing: ['Upper Respiratory Infection'],
  Coughing: ['Kennel Cough'],
  'Nasal/eye discharge': ['Feline Herpesvirus', 'Flu'],
  Fever: ['Infection', 'Viral Illness'],
  Dehydration: ['Diabetes', 'Heat Stroke'],
  'Weight loss': ['Hyperthyroidism', 'Diabetes'],
  'Labored breathing': ['Asthma', 'Heartworm'],
  'Pale or yellow gums': ['Anemia', 'Liver Disease'],
  'Excessive drooling': ['Dental Disease', 'Nausea'],
  'Abdominal pain': ['Pancreatitis', 'Constipation'],
  'Hiding behavior': ['Stress', 'Pain'],
  'Changes in urination': ['UTI', 'Kidney Stones'],
}

watch(ex4, () => {
  const illnesses = new Set()
  ex4.value.forEach((symptom) => {
    const matched = symptomIllnessMap[symptom]
    if (matched) {
      matched.forEach((illness) => illnesses.add(illness))
    }
  })
  possibleIllnesses.value = Array.from(illnesses)
})
</script>

<template>
  <v-app>
    <v-main>
      <!-- Sidebar -->
      <v-navigation-drawer v-model="drawer" app permanent width="250">
        <v-container class="text-center mt-4">
          <img :src="imgWel" alt="Welcome Icon" style="height: 100px; width: auto" />
          <h1 class="text-h5 font-weight-bold custom-title">Welcome Owner</h1>
        </v-container>

        <v-list dense>
          <v-list-item to="/layout" component="RouterLink" class="menu-item">
            <v-list-item-title>Profile</v-list-item-title>
          </v-list-item>

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

      <!-- Toggle Button -->
      <v-btn icon @click="drawer = !drawer" class="ma-2">
        <v-icon>mdi-menu</v-icon>
      </v-btn>

      <!-- Main Section -->
      <v-container fluid style="max-height: calc(100vh - 80px); overflow-y: auto">
        <v-row>
          <v-col cols="12" class="second-column-background">
            <v-row>
              <v-col cols="12" md="5">
                <v-card class="pa-6" style="height: 500px; overflow-y: auto">
                  <v-row>
                    <v-col cols="12" v-for="(symptom, i) in symptoms" :key="i">
                      <v-checkbox v-model="ex4" :label="symptom" :value="symptom" color="warning" />
                    </v-col>
                  </v-row>
                </v-card>
              </v-col>

              <v-col cols="12" md="7" class="d-flex justify-center align-center">
                <img :src="imgCatt" alt="Cat Gif" style="height: 500px; width: auto" />
              </v-col>
            </v-row>
          </v-col>
        </v-row>

        <!-- New Illness Row -->
        <v-row v-if="possibleIllnesses.length" class="mt-6 d-flex justify-center">
          <v-col cols="12" md="8">
            <v-card class="symptom-card">
              <v-card-title class="symptom-title">Possible Illnesses</v-card-title>
              <v-card-text>
                <v-chip
                  v-for="(illness, index) in possibleIllnesses"
                  :key="index"
                  color="pink lighten-4"
                  class="ma-2"
                  label
                >
                  {{ illness }}
                </v-chip>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
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

.symptom-card {
  border-radius: 12px;
  padding: 24px;
  background-color: #fff9fc;
  border: 1px solid #f0d7e9;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease;
}

.symptom-card:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.08);
}

.symptom-title {
  font-family: 'Lora', serif;
  font-size: 20px;
  font-weight: 700;
  color: #7b466a;
}

.symptom-description {
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  color: #595959;
  line-height: 1.5;
}

.symptom-suggestion-title {
  font-weight: 600;
  font-size: 16px;
  margin-bottom: 4px;
  color: #5d3c64;
}

.symptom-suggestion {
  font-family: 'Roboto', sans-serif;
  font-size: 15px;
  color: #4e4e4e;
  line-height: 1.5;
}
</style>

<link
  href="https://fonts.googleapis.com/css2?family=Lora:wght@700&family=Roboto:wght@400&display=swap"
  rel="stylesheet"
/>
