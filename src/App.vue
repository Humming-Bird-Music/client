<template>
  <div>
    <div v-if="isLogin" class="container">
      <div class="columns">
        <div class="column is-one-fifth">
          <SideBar @browse="browseBtn" @upload="uploadBtn" @c-is-login="checkLogin"></SideBar>
        </div>
        <div class="column" style="border: 0px solid red">
          <div v-if="isBrowse">
            <SearchBar></SearchBar>
            <b-loading
              :is-full-page="isFullPage"
              :active.sync="isLoadingMusics"
              :can-cancel="true"
            ></b-loading>
            <Home v-if="!isLoadingMusics" :musics="musics"></Home>
          </div>
          <div v-if="isUpload" style="margin-top: 140px;">
            <UploadMusic @uploaded="afterUpload"></UploadMusic>
          </div>
        </div>
      </div>
    </div>

    <div v-if="!isLogin" class="container" style="margin: 5% 15% 0">
      <AuthForm @c-is-login="checkLogin"></AuthForm>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import swal from 'sweetalert2'

import SideBar from './components/SideBar.vue'
import AuthForm from './components/forms/AuthForm.vue'
import SearchBar from './components/SearchBar.vue'
import Home from './components/Home.vue'
import UploadMusic from './components/forms/UploadMusic.vue'

export default {
  name: 'App',
  components: {
    SideBar,
    AuthForm,
    SearchBar,
    Home,
    UploadMusic
  },
  data() {
    return {
      isLogin: true,
      isFullPage: true,
      isLoadingMusics: true,
      isBrowse: true,
      isUpload: true,
      musics: [],
      musicsUser: []
    }
  },
  methods: {
    fetchMusic() {
      axios({
        method: 'get',
        // url: 'http://localhost:3000/musics'
        url: 'http://humming-bird.crowfx.online/musics'
      })
        .then(({ data: musics }) => {
          console.log(musics)
          this.musics = musics
          this.isLoadingMusics = false
        })
        .catch(err => {
          swal.fire({
            title: `${err.response.data.message}`,
            showCloseButton: true
          })
        })
    },
    fetchByUser(){
      axios({
        method: 'get',
        // url: 'http://localhost:3000/musics'
        url: `http://humming-bird.crowfx.online/musics/user`,
        headers: {
          authorization: localStorage.getItem('token')
        }
      })
        .then(({ data: musics }) => {
          console.log(musics)
          this.musicsUser = musics
        })
        .catch(err => {
          swal.fire({
            title: `${err.response.data.message}`,
            showCloseButton: true
          })
        })
    },
    checkLogin(e) {
      console.log(e)
      this.isLogin = e
    },
    browseBtn(e) {
      this.isBrowse = e
      this.isUpload = !e
      this.fetchMusic()
    },
    uploadBtn(e) {
      this.isBrowse = !e
      this.isUpload = e
    },
    afterUpload(e) {
      this.browseBtn(e)
    }
  },
  mounted() {
    this.fetchMusic()
    this.fetchByUser()
    if (localStorage.getItem('token')) {
      this.isLogin = true
      this.isUpload = false
    } else {
      this.isLogin = false
    }
  }
}
</script>

<style></style>
