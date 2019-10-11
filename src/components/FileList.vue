<template>
  <div class="box">
    <aplayer
      style="box-shadow: none; -webkit-box-shadow: none"
      mutex="mutex"
      :music="{
        title: music.title,
        artist: music.artist,
        src: music.url,
        pic: this.image
      }"
    >
    </aplayer>
    <nav class="level is-mobile">
      <div class="level-left">
        <b-dropdown aria-role="list" class="share-bottom">
          <span class="is-primary" slot="trigger">
            <span class="icon is-small">
              <i class="fas fa-share" aria-hidden="true"></i>
            </span>
          </span>

          <b-dropdown-item aria-role="listitem">
            <div>
              <a
                style="text-align: left"
                class="level-item"
                target="_blank"
                :href="
                  'https://twitter.com/intent/tweet?text=' +
                    music.title +
                    ' - ' +
                    music.artist +
                    '&url=' +
                    music.url
                "
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
import { parse } from 'id3-parser'
import { convertFileToBuffer, fetchFileAsBuffer } from 'id3-parser/lib/universal/helpers'
import universalParse from 'id3-parser/lib/universal'

function arrayBufferToBase64(buffer) {
  let binary = ''
  const bytes = new Uint8Array(buffer)
  const len = bytes.byteLength
  for (var i = 0; i < len; i++) {
    binary += String.fromCharCode(bytes[i])
  }
  return window.btoa(binary)
}

export default {
  name: 'FileList',
  components: {
    aplayer
  },
  props: ['music'],
  data() {
    return {
      mutex: true,
      image: null
    }
  },
  mounted() {
    fetchFileAsBuffer('https://cors-anywhere.herokuapp.com/' + this.music.url)
      .then(parse)
      .then(tag => {
        if (tag.image) {
          let encoded = arrayBufferToBase64(tag.image.data)
          this.image = `data:${tag.image.mime};base64,${encoded}`
        }
      })
  }
}
</script>

<style scoped>
.share-bottom {
  margin: 5px;
  cursor: pointer;
}
.share-bottom:hover {
  color: #7957d5;
}
</style>
