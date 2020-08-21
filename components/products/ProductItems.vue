<template>
  <div>
    <v-list-item :key="keyProd">
      <v-list-item-avatar size="60">
        <v-img :src="loadMainImage" min-height="100"></v-img>
      </v-list-item-avatar>

      <v-list-item-content>
        <v-list-item-title>
          {{ name }}
          <v-chip class="ma-2" small color="indigo" text-color="white">
            <v-avatar left>
              <v-icon>mdi-cash-multiple</v-icon>
            </v-avatar>
            <span v-html="price_html"></span>
          </v-chip>
        </v-list-item-title>
        <v-list-item-subtitle>
          <span v-html="shortDescription"></span>
        </v-list-item-subtitle>
      </v-list-item-content>
      <v-list-item-icon>
        <v-icon color="indigo accent-4" large>mdi-card-search</v-icon>
      </v-list-item-icon>
    </v-list-item>
    <v-divider :key="dividerKey"></v-divider>
  </div>
</template>


<script>
import Vue, { PropOptions } from 'vue'

export default {
  props: {
    name: {
      type: String,
      required: true,
    },
    keyProd: {
      type: Number,
      required: true,
    },
    shortDescription: {
      type: String,
      required: false,
    },
    price_html: {
      type: String,
      required: false,
    },
    qty: {
      type: String,
      required: false,
    },
    mainImage: {
      type: Object,
      required: false,
    },
  },
  created() {},
  computed: {
    loadMainImage() {
      return this.mapImage(this.mainImage)
    },
    dividerKey() {
      return `divider_` + `$this.keyProd`
    },
  },
  methods: {
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