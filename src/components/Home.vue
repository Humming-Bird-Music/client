<template>
  <div>
    <div v-if="!musics.length" style="margin: auto; text-align: center">No data</div>

    <div
      v-if="musics.length"
      style="flex-wrap: wrap; margin-top: -35px;"
      class="columns is-desktop"
    >
      <div class="column is-half" v-for="music in musics" :key="music._id">
        <FileList :key="music._id" :music="music"></FileList>
      </div>
    </div>
  </div>
</template>

<script>
import FileList from '../components/FileList.vue'

export default {
  name: 'Home',
  props: ['musics'],
  components: {
    FileList
  },
  created() {}
}
</script>
