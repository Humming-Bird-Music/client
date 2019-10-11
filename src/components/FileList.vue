<template>
  <div>
    <aplayer
      style="padding: 1.5rem"
      mutex="mutex"
      :music="{
        title: music.title,
        artist: music.artist,
        src: music.url,
        pic: this.image
      }"
    ></aplayer>
  </div>
</template>

<script>
import aplayer from 'vue-aplayer'
import { parse } from 'id3-parser'
import { convertFileToBuffer, fetchFileAsBuffer } from 'id3-parser/lib/universal/helpers'
import universalParse from 'id3-parser/lib/universal'

function arrayBufferToBase64(buffer) {
  let binary = ''
  const bytes = new Uint8Array(buffer)
  const len = bytes.byteLength
  for (var i = 0; i < len; i++) {
    binary += String.fromCharCode(bytes[i])
  }
  return window.btoa(binary)
}

export default {
  name: 'FileList',
  components: {
    aplayer
  },
  props: ['music'],
  data() {
    return {
      mutex: true,
      image: null
    }
  },
  mounted() {
    fetchFileAsBuffer('https://cors-anywhere.herokuapp.com/' + this.music.url)
      .then(parse)
      .then(tag => {
        if (tag.image) {
          let encoded = arrayBufferToBase64(tag.image.data)
          this.image = `data:${tag.image.mime};base64,${encoded}`
        }
      })
  }
}
</script>
