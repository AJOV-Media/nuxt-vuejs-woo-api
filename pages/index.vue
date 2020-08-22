<template>
  <div>
    <v-layout wrap justify-center align-center>
      <v-flex xs12 sm12 md12>
        <v-card class="mx-auto">
          <v-list three-line>
            <v-subheader>Current Products</v-subheader>
            <ProductItems
              v-for="item in products"
              :key="item.id"
              :keyProd="item.id"
              :product="item"
              @view-product-item="viewProductItem"
            />
          </v-list>
        </v-card>
      </v-flex>
      <ProductDetails :product="productForDetails" v-model="showProductDetails" />
    </v-layout>
    <v-card v-intersect="infiniteScrolling">
      <v-divider></v-divider>
      <v-card-text>
        <v-progress-circular :size="70" :width="7" :v-show="isMore" color="amber" indeterminate></v-progress-circular>Loading Product Please Wait....
      </v-card-text>

      <v-divider></v-divider>
    </v-card>
  </div>
</template>

<script>
import ProductItems from '~/components/products/ProductItems.vue'
import WooCommerceRestApi from '@woocommerce/woocommerce-rest-api'
import ProductDetails from '~/components/modals/ProductDetails.vue'

export default {
  data: () => ({
    WooCommerce: {},
    products: [],
    productForDetails: {},
    showProductDetails: false,
    currentPage: 0,
    isMore: false,
  }),
  components: {
    ProductItems,
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
  },
  methods: {
    fetchProducts(currentPage) {
      this.WooCommerce.get('products', { page: currentPage })
        .then((response) => {
          Object.keys(response.data).forEach((key) => {
            this.products = [...this.products, response.data[key]]
          })
        })
        .catch((error) => {
          console.log('Error Data:', error)
        })
        .finally(() => {
          this.isMore = false
        })
    },
    viewProductItem(e) {
      this.showProductDetails = true
      this.productForDetails = e
    },
    infiniteScrolling(entries, observer, isIntersecting) {
      if (isIntersecting) {
        this.isMore = true
        setTimeout(() => {
          this.currentPage++
          this.fetchProducts(this.currentPage)
        }, 500)
      } else {
        this.isMore = false
      }
    },
  },
}
</script>
