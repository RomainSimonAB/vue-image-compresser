<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-form @submit.prevent="handleForm">
          <v-row>
            <v-col cols="12" sm="10">
              <v-file-input v-model="file" filled dense hide-details />
            </v-col>
            <v-col cols="12" sm="2">
              <v-btn type="submit" color="primary" elevation="0" block>
                Valider
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-col>
      <v-col v-if="fileBase64" cols="12">
        <v-row>
          <v-col cols="12" sm="6">
            <img :src="fileBase64" width="100%">
          </v-col>
          <v-col cols="12" sm="6">
            <p>
              <strong>Original File Size :</strong>
              {{ (file.size / 1000000).toFixed(2) }} Mb
            </p>
            <v-btn type="button" color="primary" elevation="0" @click="handleCompress">
              Compresser
            </v-btn>
          </v-col>
        </v-row>
      </v-col>
      <v-col v-if="compressedFileBase64" cols="12">
        <v-row>
          <v-col cols="12" sm="6">
            <img :src="compressedFileBase64" width="100%">
          </v-col>
          <v-col cols="12" sm="6">
            <p>
              <strong>Compressed File Size :</strong>
              {{ (compressedFile.size / 1000000).toFixed(2) }} Mb
            </p>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data () {
    return {
      file: null,
      fileBase64: null,

      compressedFile: null,
      compressedFileBase64: null
    }
  },
  methods: {
    async handleForm () {
      this.fileBase64 = await this.toBase64(this.file)
    },
    async handleCompress () {
      const Compress = require('client-compress')

      const options = {
        targetSize: 0.2,
        quality: 0.75,
        maxWidth: 1920,
        maxHeight: 1080
      }

      const compress = new Compress(options)

      const compressedData = await compress.compress([this.file], {
        size: 0.2, // the max size in MB, defaults to 2MB
        quality: 1, // the quality of the image, max is 1,
        maxWidth: 1920, // the max width of the output image, defaults to 1920px
        maxHeight: 1080, // the max height of the output image, defaults to 1920px
        resize: true, // defaults to true, set false if you do not want to resize the image width and height
        rotate: false // See the rotation section below
      })

      this.compressedFile = compressedData[0].photo.data
      this.compressedFileBase64 = await this.toBase64(this.compressedFile)
    },
    toBase64 (file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = () => resolve(reader.result)
        reader.onerror = error => reject(error)
      })
    }
  }
}
</script>
