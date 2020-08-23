<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-card max-width="344" class="mx-auto">
        <v-list-item>
          <v-list-item-avatar color="grey"></v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title class="headline">About the site</v-list-item-title>
            <v-list-item-subtitle>demo nuxt with vuetifyjs</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-img src="https://cdn.vuetifyjs.com/images/cards/mountain.jpg" height="194"></v-img>

        <v-card-text>This page was got from https://vuetifyjs.com/en/components/cards/#cards, exploring routing of vuetify</v-card-text>

        <v-card-actions>
          <v-btn text color="deep-purple accent-4">Read</v-btn>
          <v-btn text color="deep-purple accent-4">Bookmark</v-btn>
          <v-spacer></v-spacer>
          <v-btn icon>
            <v-icon>mdi-heart</v-icon>
          </v-btn>
          <v-btn icon>
            <v-icon>mdi-share-variant</v-icon>
          </v-btn>
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