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

      <v-alert
        v-model="showCartMessage"
        dismissible
        close-icon="mdi-delete"
        color="cyan"
        border="left"
        elevation="2"
        colored-border
        icon="mdi-cart"
      >
        <span v-html="cartMessage"></span>
      </v-alert>

      <v-card-actions>
        <v-text-field v-model="amount" label="Amount" outlined type="number"></v-text-field>
        <v-btn color="indigo" @click="addToCart(product.id)">Add to Cart</v-btn>

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
      showCartMessage: false,
      cartMessage: '',
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
  methods: {
    addToCart(productId) {
      let retrieveCartObjects

      retrieveCartObjects = localStorage.getItem('wooNuxtVueCart')
      let cartObjects = JSON.parse(retrieveCartObjects || '[]')

      if (cartObjects.length > 0) {
        let updateCartObject = {}
        let updatedCartObjects = JSON.parse('[]')
        let alreadyAdded = false
        for (var i = 0; i < cartObjects.length; i++) {
          if (cartObjects[i].product_id === productId) {
            //if product id is already on the cart
            alreadyAdded = true
            updateCartObject = {
              product_id: cartObjects[i].product_id,
              howMany: 5,
            }
          } else {
            updateCartObject = {
              product_id: cartObjects[i].product_id,
              howMany: cartObjects[i].howMany,
            }
          }
          updatedCartObjects.push(updateCartObject)
        }
        if (!alreadyAdded) {
          updateCartObject = {
            product_id: productId,
            howMany: 1,
          }
          updatedCartObjects.push(updateCartObject)
          alreadyAdded = false
        }
        localStorage.setItem(
          'wooNuxtVueCart',
          JSON.stringify(updatedCartObjects)
        )
      } else {
        //only if cart is all empty
        let addCartObject = { product_id: productId, howMany: 1 }
        cartObjects.push(addCartObject)
        localStorage.setItem('wooNuxtVueCart', JSON.stringify(cartObjects))
      }

      this.cartMessage = '<stromg>Product</strong> Added to your cart'
      this.showCartMessage = true
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