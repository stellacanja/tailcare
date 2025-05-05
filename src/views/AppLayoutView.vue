<template>
  <v-responsive>
    <v-app :theme="vuetifyTheme">
      <!-- App Bar -->
      <v-app-bar
        class="px-3"
        :color="
          theme === 'light'
            ? 'red-lighten-2'
            : theme === 'dark'
              ? 'red-darken-4'
              : 'deep-purple-darken-3'
        "
        border
      >
        <v-app-bar-nav-icon
          v-if="props.isWithAppBarNavIcon"
          icon="mdi-menu"
          @click="emit('isDrawerVisible')"
        ></v-app-bar-nav-icon>

        <v-app-bar-title>
          <v-img src="/images/logo-shop.png" :width="xs ? '100%' : sm ? '40%' : '14%'"></v-img>
        </v-app-bar-title>

        <v-spacer></v-spacer>

        <!-- Theme toggle button -->
        <v-btn
          class="me-2"
          :icon="
            theme === 'light'
              ? 'mdi-weather-sunny'
              : theme === 'dark'
                ? 'mdi-weather-night'
                : 'mdi-palette'
          "
          variant="elevated"
          slim
          @click="changeTheme"
        ></v-btn>

        <TopProfileNavigation v-if="isLoggedIn" />
      </v-app-bar>

      <!-- Navigation slot -->
      <slot name="navigation"></slot>

      <!-- Main content -->
      <v-main>
        <slot name="content"></slot>
      </v-main>

      <!-- Footer -->
      <v-footer border app class="justify-center" style="background-color: #f4f5f7">
        TailCare@2025
      </v-footer>

      <!-- Bottom nav for mobile -->
      <BottomNavigation v-if="isMobileLogged" :theme="theme" />
    </v-app>
  </v-responsive>
</template>

<script setup>
import { ref, onMounted, computed, watchEffect } from 'vue'
import { useDisplay } from 'vuetify'

const props = defineProps(['isWithAppBarNavIcon'])
const emit = defineEmits(['isDrawerVisible', 'theme'])

const { xs, sm, mobile } = useDisplay()

const authStore = useAuthUserStore()
const isLoggedIn = ref(false)
const isMobileLogged = ref(false)
const isDesktop = ref(false)

const theme = ref(localStorage.getItem('theme') ?? 'light')

const themes = {
  light: { '--second-column-bg': '#f5d5e0' },
  dark: { '--second-column-bg': '#210635' },
  custom: { '--second-column-bg': '#420d4b' },
}

function changeTheme() {
  theme.value = theme.value === 'light' ? 'dark' : theme.value === 'dark' ? 'custom' : 'light'

  localStorage.setItem('theme', theme.value)
  emit('theme', theme.value)
}

// Vuetify can only use predefined themes
const vuetifyTheme = computed(() => (theme.value === 'custom' ? 'light' : theme.value))

// Apply theme variables to document root
watchEffect(() => {
  const selectedTheme = themes[theme.value]
  Object.entries(selectedTheme).forEach(([key, val]) => {
    document.documentElement.style.setProperty(key, val)
  })
})

onMounted(async () => {
  isLoggedIn.value = await authStore.isAuthenticated()
  isMobileLogged.value = mobile.value && isLoggedIn.value
  isDesktop.value = !mobile.value
})
</script>

<style scoped>
:root {
  --second-column-bg: #f5d5e0;
}

body {
  font-family: 'Roboto', sans-serif;
}
</style>
