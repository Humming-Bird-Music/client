<template>
  <div>
    <b-navbar style="margin-bottom: 3rem">
      <template slot="end">
        <section>
          <form @submit.prevent="handleSearch">
            <b-field label="Find Music">
              <b-input rounded placeholder="e.g. Poison Snail" icon="magnify" v-model="q"></b-input>
            </b-field>
          </form>
        </section>
      </template>
    </b-navbar>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      q: ''
    }
  },
  methods: {
    handleSearch() {
      console.log(this.q)
      this.$emit('q', this.q)
    }
  }
}
</script>
