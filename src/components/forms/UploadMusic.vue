<template>
  <section>
    <h1 class="title">Upload Music</h1>
    <form @submit.prevent="handleUpload" enctype="multipart/form-data">
      <b-field label="Title">
        <b-input v-model="title" type="text" placeholder="Poison Snail" maxlength="30"></b-input>
      </b-field>

      <b-field label="Artist">
        <b-input v-model="artist" type="text" placeholder="Sinta ft. Jojo" maxlength="30"></b-input>
      </b-field>

      <b-field label="Album">
        <b-input v-model="album" placeholder="Sausage So Nice" type="text" maxlength="30"></b-input>
      </b-field>

      <section>
        <!-- <b-field> -->
        <!--   <b-upload drag-drop> -->
        <!--     <section class="section"> -->
        <!--       <div class="content has-text-centered"> -->
        <!--         <p> -->
        <!--           <b-icon icon="upload" size="is-large"></b-icon> -->
        <!--         </p> -->
        <!--         <p>Drop your files here or click to upload</p> -->
        <!--       </div> -->
        <!--     </section> -->
        <!--   </b-upload> -->
        <!-- </b-field> -->

        <input type="file" ref="file" @change="onSelect" />

        <div class="tags" v-if="music.name">
          <span class="tag is-primary">
            {{ music.name }}
            <button class="delete is-small" type="button" @click="deleteDropFile"></button>
          </span>
        </div>
      </section>

      <div class="buttons">
        <b-button native-type="submit" type="is-info">Upload</b-button>
      </div>
    </form>
  </section>
</template>

<script>
import axios from 'axios'
import swal from 'sweetalert2'

export default {
  name: 'UploadMusic',
  data() {
    return {
      title: '',
      artist: '',
      album: '',
      music: ''
    }
  },
  methods: {
    deleteDropFile() {
      this.music = []
    },
    onSelect() {
      const file = this.$refs.file.files[0]
      console.log(file)
      this.music = file
    },
    handleUpload() {
      console.log(this.music.name)
      console.log('asdfasfd', this.music[0])
      let formData = new FormData()
      formData.append('file', 'tai')
      console.log(formData)
      axios({
        method: 'post',
        url: 'http://humming-bird.crowfx.online/musics',
        data: {
          title: this.title,
          artist: this.artist,
          album: this.album,
          music: formData
        },
        headers: {
          authorization: localStorage.getItem('token')
        }
      })
        .then(({ data }) => {
          console.log(data)
          swal.fire({
            title: 'Uploaded Successfully',
            timer: 1200
          })
          this.$emit('uploaded', true)
        })
        .catch(err => {
          console.log(err.response)
          swal.fire({
            title: `${err.response.data.message}`,
            showCloseButton: true
          })
        })
    }
  }
}
</script>
