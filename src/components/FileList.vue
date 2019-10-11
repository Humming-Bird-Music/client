<template>
  <div class="box">
    <aplayer
      style="box-shadow: none; -webkit-box-shadow: none"
      mutex="mutex"
      :music="{
        title: music.title,
        artist: music.artist,
        src: music.url
      }"
    >
    </aplayer>
    <nav class="level is-mobile">
      <div class="level-left">
        <b-dropdown aria-role="list">
          <span class="is-primary" slot="trigger">
            <span class="icon is-small">
              <i class="fas fa-share" aria-hidden="true"></i>
            </span>
            <!-- <b-icon icon="menu-down"></b-icon> -->
          </span>

          <b-dropdown-item aria-role="listitem">
            <div>
              <a
                style="text-align: left"
                class="level-item"
                target="_blank"
                :href="'https://twitter.com/intent/tweet?text=Just%20Share&url=' + music.url"
                onclick="javascript:window.open(this.href, '', 'menubar=no,toolbar=no,resizable=yes,scrollbars=yes,height=600,width=600');return false;"
              >
                <span style="text-align: left">
                  Twitter
                </span>
              </a>
            </div>
          </b-dropdown-item>

          <b-dropdown-item aria-role="listitem">
            <div
              class="fb-share-button"
              data-href="https://developers.facebook.com/docs/plugins/"
              data-layout="button"
              data-size="small"
            >
              <a
                style="text-align: left"
                target="_blank"
                :href="'https://www.facebook.com/sharer/sharer.php?u=' + music.url"
                class="fb-xfbml-parse-ignore"
                onclick="javascript:window.open(this.href, '', 'menubar=no,toolbar=no,resizable=yes,scrollbars=yes,height=600,width=600');return false;"
              >
                <span style="text-align: left">
                  Facebook
                </span>
              </a>
            </div>
          </b-dropdown-item>
        </b-dropdown>
      </div>
    </nav>
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
