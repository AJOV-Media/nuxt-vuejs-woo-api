<template>
  <div>
    <v-layout wrap justify-center align-center>
      <v-flex xs12 sm12 md12>
        <v-card class="mx-auto">
          <v-list three-line>
            <v-subheader>Cart</v-subheader>
            <ProductItems
              v-for="item in products"
              :key="item.id"
              :keyProd="item.id"
              :product="item"
              @view-product-item="viewProductItem"
            />
            <v-divider></v-divider>
            <v-subheader>Total</v-subheader>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title>$ {{ totalPrice }}</v-list-item-title>
                <v-list-item-subtitle>Lorem ipsum</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-flex>
      <ProductDetails :product="productForDetails" v-model="showProductDetails" />
    </v-layout>
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
    totalPrice: 0,
    prevProdCount: 0,
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

    let retrieveCartObjects

    retrieveCartObjects = localStorage.getItem('wooNuxtVueCart')
    let cartObjects = JSON.parse(retrieveCartObjects || '[]')

    if (cartObjects.length > 0) {
      for (var i = 0; i < cartObjects.length; i++) {
        let howMany = cartObjects[i].howMany
        this.WooCommerce.get('products/' + cartObjects[i].product_id)
          .then((response) => {
            response.data.qty = howMany
            this.totalPrice += Number(response.data.price) * response.data.qty

            this.products = [...this.products, response.data]
          })
          .catch((error) => {
            console.log('Error Data:', error)
          })
          .finally(() => {
            //Add Loader?
          })
      }
    }
  },
  methods: {
    viewProductItem(e) {
      this.showProductDetails = true
      this.productForDetails = e
    },
  },
}
</script>
