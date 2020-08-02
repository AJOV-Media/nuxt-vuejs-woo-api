<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-card class="mx-auto">
        <v-list three-line>
          <v-subheader>Current Products</v-subheader>
          <template v-for="(item, index) in products">
            <product-items
              :key="index"
              :keyProd="index"
              :name="item.name"
              :shortDescription="item.short_description"
              :price_html="item.price_html"
              :mainImage="item.images[0]"
            />
          </template>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script lang="ts">
import ProductItems from '~/components/products/ProductItems.vue'
import WooCommerceRestApi from '@woocommerce/woocommerce-rest-api'

export default {
  data: () => ({
    WooCommerce: {},
    products: [],
  }),
  components: {
    'product-items': ProductItems,
  },
  created() {
    this.WooCommerce = new WooCommerceRestApi({
      url: process.env.wooURL,
      consumerKey: process.env.consumerKey,
      consumerSecret: process.env.consumerSecret,
      version: process.env.wooVersion,
      verifySsl: process.env.verifySSL,
      queryStringAuth: process.env.queryStringAuth,
    })

    this.fetchProducts()
  },
  methods: {
    fetchProducts() {
      this.WooCommerce.get('products', { page: 1 })
        .then((response) => {
          Object.keys(response.data).forEach((key) => {
            this.products = [...this.products, response.data[key]]
          })
        })
        .catch((error) => {
          console.log('Error Data:', error)
        })
        .finally(() => {
          console.log(this.products)
        })
    },
  },
}
</script>
