<template>
  <v-dialog v-model="dialog" width="420">
    <v-card class="mx-auto" max-width="420">
      <v-carousel cycle height="420" hide-delimiter-background show-arrows-on-hover>
        <v-carousel-item v-for="(item, i) in product.images" :key="i" :src="item.src"></v-carousel-item>
      </v-carousel>

      <v-card-title>
        {{ product.name }}
        <v-chip class="ma-2" color="white" text-color="indigo" label>
          <v-icon left>mdi-money</v-icon>
          <span v-html="product.price_html"></span>
        </v-chip>
      </v-card-title>

      <v-card-subtitle>
        <v-chip
          class="ma-2 categ-chip"
          color="indigo"
          text-color="white"
          v-for="(item, i) in product.categories"
          :key="i"
          small
        >
          <v-icon left small>mdi-tshirt-crew</v-icon>
          {{ item.name }}
        </v-chip>
      </v-card-subtitle>

      <v-card-subtitle></v-card-subtitle>

      <v-card-actions>
        <v-text-field v-model="amount" label="Amount" outlined type="number"></v-text-field>
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
      amount: 0,
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
.v-application .categ-chip {
  margin-left: 0px !important;
}
.v-text-field {
  width: 50px;
  font-size: 12px;
  height: 50px;
  margin-right: 10px;
}
.v-text-field input {
  font-size: 12px;
}
.v-text-field label {
  font-size: 12px;
}
</style>