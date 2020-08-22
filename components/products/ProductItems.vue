<template>
  <div>
    <v-list-item :key="keyProd">
      <v-list-item-avatar size="60">
        <v-img :src="loadMainImage" min-height="100"></v-img>
      </v-list-item-avatar>

      <v-list-item-content>
        <v-list-item-title>
          {{ product.name }}
          <v-chip class="ma-2" small color="indigo" text-color="white">
            <v-avatar left>
              <v-icon>mdi-cash-multiple</v-icon>
            </v-avatar>
            <span v-html="product.price_html"></span>
          </v-chip>
        </v-list-item-title>
        <v-list-item-subtitle>
          <span v-html="product.short_description"></span>
        </v-list-item-subtitle>
      </v-list-item-content>
      <v-list-item-icon>
        <v-btn icon dark @click="viewProduct(product)">
          <v-icon color="indigo accent-4" large>mdi-card-search</v-icon>
        </v-btn>
      </v-list-item-icon>
    </v-list-item>
    <v-divider :key="dividerKey"></v-divider>
  </div>
</template>


<script>
import Vue, { PropOptions } from 'vue'

export default {
  props: {
    product: {
      type: Object,
    },
    keyProd: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      showProduct: false,
    }
  },
  created() {},
  computed: {
    loadMainImage() {
      return this.mapImage(this.product.images[0])
    },
    dividerKey() {
      return `divider_` + `$this.keyProd`
    },
  },
  methods: {
    viewProduct(item) {
      this.$emit('view-product-item', item)
    },
    mapImage(image) {
      let imagePath = ''

      if (image === undefined) {
        imagePath = 'noimagepath'
      } else {
        let imageName = image.src.split('/').slice(-1)[0]
        let imageExplode = imageName.split('.')
        let imageRename = imageExplode[0] + '-100x100.' + imageExplode[1]

        let index = image.src.split('/').indexOf(imageName)

        let urlArray = image.src.split('/')

        urlArray.splice(index, 1)

        let finalUrl = urlArray.join('/') + '/' + imageRename

        imagePath = finalUrl
      }

      return imagePath
    },
  },
}
</script>