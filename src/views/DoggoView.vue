<script setup>
import { ref, watchEffect } from 'vue'
import imgDog from '@/assets/images/doggo.gif'
import imgWel from '@/assets/images/welcome.png'

// Initializing theme and other states
const theme = ref('light')
const consultOpen = ref(false) // Toggles visibility of Consult section
const typeOpen = ref(false) // Toggles visibility of the "Type" options (Cat and Dog)
const currentTime = ref(new Date().toLocaleString())

// Theme settings with the same colors as before
const themes = {
  light: {
    '--navbar-bg': '#f5d5e0', // Beige background
  },
  dark: {
    '--navbar-bg': '#210635', // Purple background
  },
  custom: {
    '--navbar-bg': '#42od4b', // Green background
  },
}

// Function to change themes
function changeTheme() {
  theme.value = theme.value === 'light' ? 'dark' : theme.value === 'dark' ? 'custom' : 'light'
}

watchEffect(() => {
  const selectedTheme = themes[theme.value]
  Object.keys(selectedTheme).forEach((key) => {
    document.documentElement.style.setProperty(key, selectedTheme[key])
  })
})

// Function to simulate sign out
function signOut() {
  alert('Signed out!')
}

// Array of headline articles with titles, summaries, links, and images
const headlines = ref([
  {
    title:
      'Glamorous dog owner gifted $35k to "say goodbye" to pet that bit her announces she\'s actually KEEPING him',
    summary:
      'A dog owner surprised everyone when she decided to keep her dog despite a biting incident.',
    link: 'https://www.dailymail.co.uk/news/article-14644101/glamorous-dog-owner-shayna-crimo-pet-bit-keeping.html',
    imgSrc:
      'https://th.bing.com/th?id=OVFT.Q-NQH9dLgneJh9HPsKQqby&pid=News&w=300&h=186&c=14&rs=2&qlt=90&dpr=1.3',
  },
  {
    title: '260 Clever Boy Dog Names',
    summary: 'The ultimate list of dog names for your boy dog.',
    link: 'https://www.dailypaws.com/dogs-puppies/dog-names/boy-dog-names',
    imgSrc:
      'https://th.bing.com/th/id/ODL.eb53ed4a16b30b7e24b8c753d355e524?w=197&h=112&c=7&rs=1&qlt=80&r=0&o=6&dpr=1.3&pid=RichNav',
  },
  {
    title: 'Absolutely Gigantic Maine Coon Cat Must Be Seen To Be Believed',
    summary: 'This Maine Coon cat is massive and simply amazing.',
    link: 'https://pethelpful.com/pet-news/maine-coon-big-as-child',
    imgSrc:
      'https://th.bing.com/th/id/ODL.9163eedf3df4aa08bcd1550685b995c1?w=197&h=112&c=7&rs=1&qlt=80&r=0&o=6&dpr=1.3&pid=RichNav',
  },
  {
    title: 'Pet Health - Fox News',
    summary: 'Stay updated on the latest pet health news.',
    link: 'https://www.foxnews.com/category/health/healthy-living/pet-health?msockid=0b80acf68d61624d2560b81f8cfc6339',
    imgSrc:
      'https://th.bing.com/th/id/OIP.L9UfDQ4n2gjwgQC4rQS74wAAAA?w=80&h=80&c=1&vt=10&bgcl=0ad481&r=0&o=6&dpr=1.3&pid=ImgRC',
  },
  {
    title: '142 of the Cutest Food Names for Dogs Who Love to Snack',
    summary: 'A fun list of food-inspired names for your furry friend.',
    link: 'https://www.dailypaws.com/dogs-puppies/dog-names/food-names-for-dogs',
    imgSrc:
      'https://th.bing.com/th/id/ODL.ff1fcd9a9268ae035260e3a16747d5ee?w=197&h=112&c=7&rs=1&qlt=80&r=0&o=6&dpr=1.3&pid=RichNav',
  },
  {
    title: 'PET NEWS - PET HELP',
    summary: 'All the latest news and updates for pet lovers.',
    link: 'https://pethelpful.com/pet-news',
    imgSrc:
      'https://th.bing.com/th/id/OIP.2mGFoXG3U2uR75QaSNPQ6AHaHa?w=80&h=80&c=1&vt=10&bgcl=346cfe&r=0&o=6&dpr=1.3&pid=ImgRC',
  },
])

// Function to open the links in a new tab
function goToLink(url) {
  window.open(url, '_blank')
}

// Function to toggle consult section visibility
function toggleConsult() {
  consultOpen.value = !consultOpen.value
}

// Function to toggle type section visibility
function toggleType() {
  typeOpen.value = !typeOpen.value
}

// Update current time every second
function updateTime() {
  setInterval(() => {
    currentTime.value = new Date().toLocaleString()
  }, 1000)
}

updateTime()
</script>

<template>
  <v-app>
    <!-- Main Layout -->
    <v-main>
      <!-- Sidebar with the image above options -->
      <v-navigation-drawer app permanent width="250">
        <!--applayout-->

        <!-- Welcome Image and Title -->
        <v-container class="text-center mt-4">
          <img :src="imgWel" alt="Welcome Icon" style="height: 100px; width: auto" />
          <h1 class="text-h5 font-weight-bold custom-title">Welcome Owner</h1>
        </v-container>

        <v-list dense>
          <!-- Dashboard Section -->
          <v-list-item to="/layout" component="RouterLink" class="menu-item">
            <v-list-item-title>Profile</v-list-item-title>
          </v-list-item>
          <v-list-item class="menu-item">
            <v-list-item-title>Dashboard</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <!-- Consult Section -->
          <v-list-item @click="toggleConsult" class="menu-item">
            <v-list-item-title>Consult</v-list-item-title>
          </v-list-item>

          <!-- Type option that appears only when Consult is clicked -->
          <v-list-item v-if="consultOpen" @click="toggleType" class="menu-item">
            <v-list-item-title>Type</v-list-item-title>
          </v-list-item>

          <!-- Cat and Dog options appear only when Type is clicked -->
          <v-list-item v-if="typeOpen" to="/symptomscat" component="RouterLink" class="menu-item">
            <v-list-item-title>Cat</v-list-item-title>
          </v-list-item>
          <v-list-item v-if="typeOpen" to="/symptomsdog" component="RouterLink" class="menu-item">
            <v-list-item-title>Dog</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <!-- Contact Us Section -->
          <v-list-item to="/contact" component="RouterLink" class="menu-item">
            <v-list-item-title>Contact Us</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <!-- Change Theme Section -->
          <v-list-item @click="changeTheme" class="menu-item">
            <v-list-item-title>Change Theme</v-list-item-title>
          </v-list-item>

          <v-divider></v-divider>

          <!-- Sign Out Section -->
          <v-list-item @click="signOut" class="menu-item">
            <v-list-item-title>Sign Out</v-list-item-title>
          </v-list-item>

          <!-- Date & Time Card -->
          <v-card class="date-time-card mt-5" style="padding: 16px; text-align: center">
            <v-card-title class="text-h6">Current Date and Time</v-card-title>
            <v-card-subtitle>
              <div>{{ currentTime }}</div>
            </v-card-subtitle>
          </v-card>
        </v-list>
      </v-navigation-drawer>

      <v-container fluid style="max-height: calc(100vh - 80px); overflow-y: auto">
        <!-- Headlines section with 'Pet News and Facts' as header -->
        <v-row>
          <v-col cols="12" class="second-column-background">
            <v-card class="pa-4" hover>
              <v-card-title class="text-h5 custom-title">Pet News and Facts</v-card-title>
              <v-divider></v-divider>
              <v-row>
                <v-col v-for="(headline, index) in headlines" :key="index" cols="12" sm="6" md="4">
                  <v-card
                    class="pa-4"
                    hover
                    @click="goToLink(headline.link)"
                    style="cursor: pointer; background-color: #8569e4"
                  >
                    <v-card-title class="text-h6">{{ headline.title }}</v-card-title>
                    <v-card-subtitle>{{ headline.summary }}</v-card-subtitle>
                    <v-img :src="headline.imgSrc" height="200px"></v-img>
                  </v-card>
                </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>

        <!-- Video Card Section -->
        <v-card max-width="800" class="video-card mx-auto mt-5">
          <v-card-title class="text-h6">Watch Our Intro Video</v-card-title>
          <v-card-subtitle>
            <v-responsive aspect-ratio="16/9">
              <iframe
                width="100%"
                height="100%"
                src="https://www.youtube.com/embed/js6IrC_6VcM"
                title="Intro Video"
                frameborder="0"
                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
              ></iframe>
            </v-responsive>
          </v-card-subtitle>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
:root {
  --navbar-bg: #f5d5e0; /* Beige color */
}

[data-theme='dark'] {
  --navbar-bg: #210635; /* Purple color */
}

[data-theme='custom'] {
  --navbar-bg: #42od4b; /* Green color */
}

/* Custom font for the Welcome Owner and Pet News titles */
.custom-title {
  font-family: 'Lora', serif;
  color: #7b466a;
  font-weight: bold;
  margin-bottom: 10px;
}

/* Style for menu items */
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

/* Optional: Global font for the main content */
body {
  font-family: 'Lora', serif;
}

/* Optional: Date and time card styles */
.date-time-card {
  background-color: #ffcccb;
  border-radius: 10px;
}

/* Second column background */
.second-column-background {
  background-color: #f7f0fa;
  border-radius: 10px;
}

/* Video Card Styles */
.video-card {
  background-color: #f2e6fa;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.video-card .v-card-title {
  font-size: 18px;
  font-weight: bold;
}
</style>
