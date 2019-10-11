<template>
  <div>
    <aplayer
      style="padding: 1.5rem"
      mutex="mutex"
      :music="{
        title: music.title,
        artist: music.artist,
        src: music.url
      }"
    ></aplayer>
  </div>
</template>

<script>
import aplayer from 'vue-aplayer'

export default {
  name: 'FileList',
  components: {
    aplayer
  },
  props: ['music'],
  data() {
    return {
      mutex: true
    }
  }
}
</script>
