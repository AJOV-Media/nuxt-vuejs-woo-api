<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-card elevation="4" light tag="section">
        <v-card-title>
          <v-layout align-center justify-space-between>
            <h3 class="headline">Woo Nuxt</h3>
            <v-flex>
              <v-img class="ml-3" contain height="48px" position="center right" src="/api-logo.png"></v-img>
            </v-flex>
          </v-layout>
        </v-card-title>
        <v-divider></v-divider>

        <v-stepper v-model="e1">
          <v-stepper-header>
            <v-stepper-step :complete="e1 > 1" step="1">Personal Information</v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step :complete="e1 > 2" step="2">Billing Information</v-stepper-step>

            <v-divider></v-divider>

            <v-stepper-step step="3">Shipping Information</v-stepper-step>
          </v-stepper-header>

          <v-stepper-items>
            <v-stepper-content step="1">
              <v-form class="mb-12">
                <v-text-field outline label="First Name" type="text" v-model="firstname"></v-text-field>
                <v-text-field outline label="Last Name" type="text" v-model="lastname"></v-text-field>
                <v-text-field outline label="Email" type="email" v-model="email"></v-text-field>
                <v-text-field outline label="Username" type="text" v-model="username"></v-text-field>
                <v-text-field outline label="Password" type="password" v-model="password"></v-text-field>
                <v-text-field
                  outline
                  label="Confirm Password"
                  type="password"
                  v-model="confirm_password"
                ></v-text-field>
              </v-form>

              <v-btn color="primary" @click="e1 = 2">Continue</v-btn>

              <v-btn text>Cancel</v-btn>
            </v-stepper-content>

            <v-stepper-content step="2">
              <v-card class="mb-12" color="grey lighten-1" height="200px"></v-card>

              <v-btn color="primary" @click="e1 = 3">Continue</v-btn>

              <v-btn text>Cancel</v-btn>
            </v-stepper-content>

            <v-stepper-content step="3">
              <v-card class="mb-12" color="grey lighten-1" height="200px"></v-card>

              <v-btn color="primary" @click="e1 = 1">Continue</v-btn>

              <v-btn text>Cancel</v-btn>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  data() {
    return {
      e1: 1,
    }
  },
}
</script>
<style scoped>
</style>