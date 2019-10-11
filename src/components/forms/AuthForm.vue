<template>
  <div class="columns">
    <div class="column is-two-fifths" style="padding: 0; height: 650px">
      <img
        src="https://images.pexels.com/photos/1337753/pexels-photo-1337753.jpeg?auto=compress&cs=tinysrgb&dpr=3&h=750&w=1260"
        style="object-fit: cover; width: 100%; height: 100%"
        alt="Image"
      />
    </div>
    <div v-if="isRegisterForm" class="box column p-5" style="border-radius: 0;">
      <div class="media">
        <div class="media-content">
          <section>
            <h1 class="title">Register</h1>
            <form @submit.prevent="handleRegister">
              <b-field label="Username">
                <b-input v-model="username" placeholder="johndoe" maxlength="30"></b-input>
              </b-field>

              <b-input style="display: none"></b-input>

              <b-field label="Email">
                <b-input v-model="email" placeholder="john@doe.com" maxlength="30"></b-input>
              </b-field>

              <b-field label="Password">
                <b-input
                  v-model="password"
                  placeholder="secret"
                  type="password"
                  maxlength="30"
                ></b-input>
              </b-field>
              <div class="buttons">
                <b-button native-type="submit" type="is-info">Register</b-button>
              </div>
            </form>
          </section>
        </div>
      </div>
      <p style="margin-top: 3rem">
        <small>Already have an account?</small>
        <b-button @click="toggleForm" size="is-small" type="is-link" outlined>Log in</b-button>
      </p>
    </div>

    <div v-if="!isRegisterForm" class="box column p-5" style="border-radius: 0;">
      <div class="media">
        <div class="media-content">
          <section>
            <h1 class="title">Login</h1>
            <form @submit.prevent="handleLogin">
              <b-field label="Username / Email">
                <b-input
                  v-model="identity"
                  placeholder="johndoe / john@doe.com"
                  maxlength="30"
                ></b-input>
              </b-field>

              <b-field label="Password">
                <b-input
                  v-model="password"
                  placeholder="secret"
                  type="password"
                  maxlength="30"
                ></b-input>
              </b-field>
              <div class="buttons">
                <b-button native-type="submit" type="is-info">Log in</b-button>
              </div>
            </form>
          </section>
        </div>
      </div>
      <p style="margin-top: 3rem">
        <small>Don't have an account?</small>
        <b-button @click="toggleForm" size="is-small" type="is-link" outlined>Register</b-button>
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import swal from 'sweetalert2'

const host = 'http://humming-bird.crowfx.online'

export default {
  name: 'RegisterForm',
  data() {
    return {
      email: '',
      username: '',
      password: '',
      identity: '',
      isRegisterForm: false
    }
  },
  methods: {
    toggleForm() {
      this.isRegisterForm = !this.isRegisterForm
    },
    handleRegister() {
      axios({
        method: 'post',
        url: `${host}/users/register`,
        data: {
          username: this.username,
          email: this.email,
          password: this.password
        }
      })
        .then(({ data }) => {
          localStorage.setItem('token', data.token)
          localStorage.setItem('username', data.username)
          localStorage.setItem('email', data.email)
          this.$emit('c-is-login', true)
        })
        .catch(err => {
          swal.fire({
            title: `${err.response.data.message.join('\n')}`,
            showCloseButton: true
          })
          this.email = ''
          this.username = ''
          this.password = ''
        })
    },
    handleLogin() {
      axios({
        method: 'post',
        url: `${host}/users/login`,
        data: {
          identity: this.identity,
          password: this.password
        }
      })
        .then(({ data }) => {
          localStorage.setItem('token', data.token)
          localStorage.setItem('username', data.username)
          localStorage.setItem('email', data.email)
          this.$emit('c-is-login', true)
        })
        .catch(err => {
          swal.fire({
            title: `${err.response.data.message}`,
            showCloseButton: true
          })
          this.identity = ''
          this.password = ''
        })
    }
  }
}
</script>

<style scoped>
.p-5 {
  padding: 5rem;
}
.border-radius {
  border-radius: 10px;
}
</style>
