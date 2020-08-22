<template>
  <v-dialog v-model="dialog" width="400">
    <v-card class="mx-auto" max-width="400">
      <v-carousel cycle height="400" hide-delimiter-background show-arrows-on-hover>
        <v-carousel-item v-for="(slide, i) in slides" :key="i">
          <v-sheet :color="colors[i]" height="100%">
            <v-row class="fill-height" align="center" justify="center">
              <div class="display-3">{{ slide }} Slide</div>
            </v-row>
          </v-sheet>
        </v-carousel-item>
      </v-carousel>

      <v-card-title>{{ product.name }}</v-card-title>

      <v-card-subtitle v-html="product.price_html"></v-card-subtitle>

      <v-card-actions>
        <v-btn color="indigo">Add to Cart</v-btn>

        <v-btn color="red" @click="dialog = false">Close</v-btn>

        <v-spacer></v-spacer>

        <v-btn icon @click="showDetails = !showDetails">
          <v-icon>{{ showDetails ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="showDetails">
          <v-divider></v-divider>

          <v-card-text v-html="product.description"></v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  props: {
    product: {
      type: Object,
      required: true,
    },
    value: Boolean,
  },
  data() {
    return {
      showDetails: false,
      colors: [
        'indigo',
        'warning',
        'pink darken-2',
        'red lighten-1',
        'deep-purple accent-4',
      ],
      slides: ['First', 'Second', 'Third', 'Fourth', 'Fifth'],
    }
  },
  computed: {
    dialog: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      },
    },
  },
}
</script>
<style scoped>
</style>