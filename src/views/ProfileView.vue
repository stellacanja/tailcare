<script setup>
import { supabase, formActionDefault } from '@/utils/supabase'
import { getAvatarText } from '@/utils/helpers'
import { useRouter } from 'vue-router'
import { onMounted, ref, watchEffect } from 'vue'
import imgWel from '@/assets/images/welcome.png'

const router = useRouter()

const theme = ref('light')
const consultOpen = ref(false)
const typeOpen = ref(false)
const currentTime = ref(new Date().toLocaleString())
const formAction = ref({ ...formActionDefault })

const themes = {
  light: {
    '--navbar-bg': '#f5d5e0',
    '--background-color': '#bb9ac9',
  },
  dark: {
    '--navbar-bg': '#210635',
    '--background-color': '#bf7ab8',
  },
  custom: {
    '--navbar-bg': '#42od4b',
    '--background-color': '#f9cbdf',
  },
}

const changeTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : theme.value === 'dark' ? 'custom' : 'light'
}

watchEffect(() => {
  const selectedTheme = themes[theme.value]
  Object.entries(selectedTheme).forEach(([key, value]) => {
    document.documentElement.style.setProperty(key, value)
  })
})

setInterval(() => {
  currentTime.value = new Date().toLocaleString()
}, 1000)

const toggleConsult = () => (consultOpen.value = !consultOpen.value)
const toggleType = () => (typeOpen.value = !typeOpen.value)

const userData = ref({
  initials: '',
  email: '',
  username: '',
  profileUrl: null,
})

const getUser = async () => {
  const {
    data: { user },
  } = await supabase.auth.getUser()

  if (user) {
    const metadata = user.user_metadata
    userData.value.email = metadata.email
    userData.value.username = metadata.username
    userData.value.initials = getAvatarText(metadata.username)
    userData.value.profileUrl = metadata.profileUrl || null
  }
}

const onLogout = async () => {
  formAction.value.formProcess = true
  const { error } = await supabase.auth.signOut()
  formAction.value.formProcess = false
  if (error) {
    console.error('Error during logout:', error)
    return
  }
  router.replace('/')
}

const handleImageUpload = async (e) => {
  const file = e.target.files[0]
  if (!file) return

  const filePath = `avatars/${Date.now()}_${file.name}`
  const { error: uploadError } = await supabase.storage
    .from('profile-pictures')
    .upload(filePath, file)

  if (uploadError) {
    console.error('Upload error:', uploadError)
    return
  }

  const { data, error: urlError } = supabase.storage.from('profile-pictures').getPublicUrl(filePath)

  if (urlError || !data?.publicUrl) {
    console.error('Error getting public URL:', urlError)
    return
  }

  const publicUrl = data.publicUrl
  userData.value.profileUrl = publicUrl

  const { error: updateError } = await supabase.auth.updateUser({
    data: { profileUrl: publicUrl },
  })

  if (updateError) {
    console.error('Error updating user metadata:', updateError)
    return
  }

  await getUser()
}

onMounted(() => {
  getUser()
})
</script>

<template>
  <v-app>
    <!-- Sidebar -->
    <v-navigation-drawer app permanent width="250">
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

        <v-list-item
          @click="onLogout"
          class="menu-item"
          :loading="formAction.formProcess"
          :disabled="formAction.formProcess"
        >
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

    <!-- Main Content -->
    <v-main class="main-background">
      <v-container class="profile-container" fluid>
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6">
            <v-card class="pa-6" elevation="10" rounded="lg">
              <div class="text-center">
                <v-avatar
                  size="110"
                  class="mx-auto mb-3 profile-avatar"
                  @click="$refs.imageInput.click()"
                >
                  <template v-if="userData.profileUrl">
                    <v-img :src="userData.profileUrl" />
                  </template>
                  <template v-else>
                    <span class="text-h5">{{ userData.initials }}</span>
                  </template>
                </v-avatar>

                <p class="text-caption grey--text mb-1">Click image to upload</p>

                <h3 class="text-h6 mt-2 font-weight-bold">{{ userData.username }}</h3>
                <p class="text-body-2 grey--text text--darken-2">{{ userData.email }}</p>

                <v-divider class="my-5" />

                <input
                  ref="imageInput"
                  type="file"
                  accept="image/*"
                  class="d-none"
                  @change="handleImageUpload"
                />

                <v-btn color="error" variant="outlined" rounded @click="onLogout"> Logout </v-btn>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped>
:root {
  --navbar-bg: #f5d5e0;
  --background-color: #bb9ac9;
}

.main-background {
  background-color: var(--background-color);
  min-height: 100vh;
  padding-left: 250px; /* Matches sidebar width */
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

.date-time-card {
  background-color: #ffcccb;
  border-radius: 10px;
}

.profile-container {
  background-color: transparent;
  padding-bottom: 60px;
  padding-top: 50px;
}

.profile-avatar {
  cursor: pointer;
  border: 2px solid #ccc;
  transition: transform 0.2s ease;
}

.profile-avatar:hover {
  transform: scale(1.05);
  border-color: #3f51b5;
}

.text-center {
  text-align: center;
}

.v-card {
  border-radius: 20px;
}

.v-btn {
  text-transform: none;
  font-weight: 500;
}
</style>
