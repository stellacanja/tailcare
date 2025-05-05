<script setup>
import { supabase, formActionDefault } from '@/utils/supabase'
import { getAvatarText } from '@/utils/helpers'
import { useRouter } from 'vue-router'
import { onMounted, ref } from 'vue'

const router = useRouter()

const userData = ref({
  initials: '',
  email: '',
  username: '',
  profileUrl: null,
})

const formAction = ref({
  ...formActionDefault,
})

const getUser = async () => {
  const { data: { user } } = await supabase.auth.getUser()

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
  const { error: uploadError } = await supabase.storage.from('profile-pictures').upload(filePath, file)

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

  // Update local display immediately
  userData.value.profileUrl = publicUrl

  // Update the user profile data in Supabase
  const { error: updateError } = await supabase.auth.updateUser({
    data: { profileUrl: publicUrl },
  })

  if (updateError) {
    console.error('Error updating user metadata:', updateError)
    return
  }

  // Optionally re-fetch user data for consistency
  await getUser()
}

onMounted(() => {
  getUser()
})
</script>

<template>
  <v-container class="profile-container" fluid>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card class="pa-6" elevation="10" rounded="lg">
          <div class="text-center">
            <!-- Profile Picture (clickable) -->
            <v-avatar size="110" class="mx-auto mb-3 profile-avatar" @click="$refs.imageInput.click()">
              <template v-if="userData.profileUrl">
                <v-img :src="userData.profileUrl" />
              </template>
              <template v-else>
                <span class="text-h5">{{ userData.initials }}</span>
              </template>
            </v-avatar>

            <p class="text-caption grey--text mb-1">Click image to upload</p>

            <!-- Info -->
            <h3 class="text-h6 mt-2 font-weight-bold">{{ userData.username }}</h3>
            <p class="text-body-2 grey--text text--darken-2">{{ userData.email }}</p>

            <v-divider class="my-5" />

            <!-- Hidden Input for Upload -->
            <input
              ref="imageInput"
              type="file"
              accept="image/*"
              class="d-none"
              @change="handleImageUpload"
            />

            <!-- Logout Button -->
            <v-btn color="error" variant="outlined" rounded @click="onLogout">
              Logout
            </v-btn>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
.profile-container {
  margin-top: 50px;
  background-color: #f7f7f7;
  padding-bottom: 60px;
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
