<template>
  <b-menu>
    <figure class="image is-96x96" style="margin: auto">
      <img src="../assets/hummingbird-logo2.png" alt="logo" />
    </figure>
    <b-menu-list label="Menu">
      <b-menu-item icon="account" @click="browse" :label="username"></b-menu-item>
      <b-menu-item icon="magnify" @click="browse" label="Browse"></b-menu-item>
    </b-menu-list>
    <b-menu-list>
      <b-menu-item label="Upload" @click="upload" icon="upload"> </b-menu-item>
    </b-menu-list>
    <b-menu-list label="Actions">
      <b-menu-item @click="manageMusic" icon="share" label="Manage Music"></b-menu-item>
      <b-menu-item
        class="menu-item"
        @click="handleLogout"
        icon="logout"
        label="Logout"
      ></b-menu-item>
    </b-menu-list>
  </b-menu>
</template>

<script>
import swal from 'sweetalert2'

export default {
  name: 'SideBar',
  props: ['username'],
  data() {
    return {}
  },
  methods: {
    handleLogout() {
      swal
        .fire({
          title: 'Are you sure?',
          type: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes'
        })
        .then(result => {
          if (result.value) {
            localStorage.removeItem('token')
            localStorage.removeItem('username')
            localStorage.removeItem('email')
            this.$emit('c-is-login', false)
            swal.fire({
              title: 'Logged out',
              timer: 1000,
              type: 'success'
            })
          }
        })
    },
    browse() {
      this.$emit('browse', true)
    },
    upload() {
      this.$emit('upload', true)
    },
    manageMusic() {
      this.$emit('manage', true)
    }
  }
}
</script>
