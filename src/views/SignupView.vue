<script setup>
import { ref } from 'vue'
import imgFour from '@/assets/images/four.png'
import imgWel from '@/assets/images/welcome.png'
</script>

<template>
  <v-responsive class="border rounded">
    <v-app>
      <!-- Centered App Bar Logo -->
      <v-app-bar class="navbar" flat height="150" style="background-color: #f4f5f7">
        <v-container class="d-flex justify-center align-center fill-height pa-0">
          <div class="d-flex flex-column align-center" style="line-height: 1; margin-top: 4px">
            <img :src="imgWel" alt="TailCare Logo" style="height: 150px; object-fit: contain" />
          </div>
        </v-container>
      </v-app-bar>

      <br />
      <br />

      <v-main>
        <v-container>
          <v-row>
            <v-col cols="5" md="5">
              <br />
              <br />
              <v-card class="pa-4">
                <h1 id="welc" style="color: #8c52ff"><b>WELCOME OWNERS</b></h1>
                <br />
                <v-form fast-fail @submit.prevent>
                  <v-text-field label="Email address"></v-text-field>

                  <v-text-field label="Password"></v-text-field>

                  <v-btn class="mt-2" type="submit" style="background-color: skyblue" block
                    >Submit</v-btn
                  >
                </v-form>
              </v-card>
              <br />
              <h3>Don't have an account? <link rel="" href="" />Sign up</h3>
            </v-col>
            <v-col cols="7" md="7">
              <img
                :src="imgFour"
                alt="Four Pets"
                class="card-img-top"
                style="height: 500px; object-fit: cover; width: 800px"
              />
            </v-col>
          </v-row>
        </v-container>
      </v-main>

      <v-footer border app id="foot" class="justify-center" style="background-color: #f4f5f7"
        >TailCare@2025</v-footer
      >
    </v-app>
  </v-responsive>
</template>
