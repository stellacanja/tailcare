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
})
const formAction = ref({
  ...formActionDefault,
})
const onLogout = async () => {
  formAction.value = { ...formActionDefault }
  formAction.value.formProcess = true

  const { error } = await supabase.auth.signOut()
  if (error) {
    console.error('Error during logout:', error)
    return
  }
  formAction.value.formProcess = false
  router.replace('/')
}

const getUser = async () => {
  const {
    data: {
      user: { user_metadata: metadata },
    },
  } = await supabase.auth.getUser()
  userData.value.email = metadata.email
  userData.value.username = metadata.username
  userData.value.initials = getAvatarText(userData.value.username)
}
</script>

<template>
  <v-container style="height: 300px" fluid>
    <v-row justify="center">
      <v-menu min-width="200px">
        <template #activator="{ props }">
          <v-btn icon v-bind="props">
            <v-avatar color="brown" size="large">
              <span class="text-h5">{{ userData.initials }}</span>
            </v-avatar>
          </v-btn>
        </template>
        <v-card>
          <v-card-text>
            <div class="mx-auto text-center">
              <v-avatar color="brown">
                <span class="text-h5">{{ userData.initials }}</span>
              </v-avatar>
              <h3>{{ user.fullName }}</h3>
              <p class="text-caption mt-1">
                {{ user.email }}
              </p>
              <v-divider class="my-3"></v-divider>
              <v-btn variant="text" rounded> Edit Account </v-btn>
              <v-divider class="my-3"></v-divider>
              <v-btn variant="text" rounded> Disconnect </v-btn>
            </div>
          </v-card-text>
        </v-card>
      </v-menu>
    </v-row>
  </v-container>
</template>
