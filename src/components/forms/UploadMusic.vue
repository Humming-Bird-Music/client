<template>
  <section>
    <h1 class="title">Upload Music</h1>
    <form @submit.prevent="handleUpload" enctype="multipart/form-data">
      <b-field label="Title">
        <b-input
          v-model="title"
          required
          type="text"
          placeholder="Poison Snail"
          maxlength="30"
        ></b-input>
      </b-field>

      <b-field label="Artist">
        <b-input
          v-model="artist"
          required
          type="text"
          placeholder="Sinta ft. Jojo"
          maxlength="30"
        ></b-input>
      </b-field>

      <b-field label="Album">
        <b-input
          v-model="album"
          required
          placeholder="Sausage So Nice"
          type="text"
          maxlength="30"
        ></b-input>
      </b-field>

      <section>
        <b-field>
          <b-upload v-model="music" drag-drop>
            <section class="section">
              <div class="content has-text-centered">
                <p>
                  <b-icon icon="upload" size="is-large"></b-icon>
                </p>
                <p>Drop your files here or click to upload</p>
              </div>
            </section>
          </b-upload>
        </b-field>

        <div class="tags" v-if="music.name">
          <span class="tag is-primary">
            {{ music.name }}
            <button class="delete is-small" type="button" @click="deleteDropFile"></button>
          </span>
        </div>
      </section>

      <div class="buttons" style="margin-top: 3rem">
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
      music: []
    }
  },
  methods: {
    deleteDropFile() {
      this.music = []
    },
    handleUpload() {
      swal.fire({
        onOpen() {
          swal.showLoading()
        }
      })
      /*this.$buefy.snackbar.open({
        // message: 'Yellow button and positioned on top, click to close',
        message: `Uploading ${this.title}...`,
        type: 'is-info',
        position: 'is-top-right',
        actionText: 'Retry',
        indefinite: true,
        onAction: () => {
          this.$buefy.toast.open({
            message: 'Action pressed',
            queue: false
          })
        }
      })*/
      const formData = new FormData()
      formData.append('music', this.music)
      formData.set('title', this.title)
      formData.set('artist', this.artist)
      formData.set('album', this.album)
      axios({
        method: 'post',
        url: 'http://humming-bird.crowfx.online/musics',
        data: formData,
        headers: {
          authorization: localStorage.getItem('token'),
          'Content-Type': 'multipart/form-data'
        }
      })
        .then(({ data }) => {
          swal.fire({
            title: 'Uploaded Successfully',
            timer: 1200
          })
          this.$emit('uploaded', true)
        })
        .catch(err => {
          swal.fire({
            title: `${err.response.data.message}`,
            showCloseButton: true
          })
        })
    }
  }
}
</script>
