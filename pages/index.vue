<template>
  <v-layout wrap justify-center align-center>
    <v-flex xs12 sm12 md12>
      <v-card class="mx-auto">
        <v-list three-line>
          <v-subheader>Current Products</v-subheader>
          <template v-for="(item, index) in products">
            <product-items :key="index" :keyProd="index" :products="item" />
          </template>
        </v-list>
      </v-card>
    </v-flex>
    <ProductDetails v-model="showProductDetails" />
  </v-layout>
</template>

<script>
import ProductItems from '~/components/products/ProductItems.vue'
import WooCommerceRestApi from '@woocommerce/woocommerce-rest-api'
import ProductDetails from '~/components/modals/ProductDetails.vue'

export default {
  data: () => ({
    WooCommerce: {},
    products: [],
    showProductDetails: true,
  }),
  components: {
    'product-items': ProductItems,
    ProductDetails,
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
