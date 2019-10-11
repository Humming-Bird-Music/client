<template>
  <div>
    <div v-if="!myMusics.length" style="margin: auto; text-align: center">No data</div>

    <div v-if="myMusics.length" style="flex-wrap: wrap" class="columns is-desktop">
      <div class="column is-half" v-for="music in myMusics">
        <FileList :key="music._id" :music="music"></FileList>
      </div>
    </div>
  </div>
</template>

<script>
import FileList from '../components/FileList.vue'

export default {
  name: 'ManageMusic',
  props: ['myMusics'],
  components: {
    FileList
  }
}
</script>
