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
        <v-card-text>
          <p>Sign in with your username and password:</p>
          <v-form>
            <v-text-field outline label="Username" type="text" v-model="username"></v-text-field>
            <v-text-field outline hide-details label="Password" type="password" v-model="password"></v-text-field>
          </v-form>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn color="indigo">Forgot password?</v-btn>
          <v-spacer></v-spacer>
          <v-btn color="indigo">Login</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>
<script>
export default {}
</script>
<style scoped>
</style>