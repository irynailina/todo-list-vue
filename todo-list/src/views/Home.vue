<template>
  <div>
    <h2>Home page</h2>
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi assumenda
      explicabo tempora sint, voluptatem earum fugit et. Iusto mollitia
      similique nemo consequuntur est sapiente blanditiis, ducimus saepe
      asperiores nobis provident?
    </p>
    <div>
      <mini-audio>
        <source
          audio-source="http://file-examples.com/wp-content/uploads/2017/11/file_example_MP3_1MG.mp3"
          type="audio/wav"
          controls
        />
      </mini-audio>
    </div>
    <router-link to="/todos">Todos</router-link>
  </div>
</template>

<script>
// import { audioPlayer } from "vue-md-player";
// import "vue-md-player/dist/vue-md-player.css";
import Vue from 'vue'
import VueAudio from 'vue-audio-better'
Vue.use(VueAudio)
export default {
};
</script>

<style scoped>
.audioPlayer {
  display: block;
  border-radius: 20px;
  margin: 0 auto;
}
.vueAudioBetter[data-v-3a8dc9f9] {
  background-color: red;
  background-image: none;
}
/* .wrap {
  width: 80%;
  min-width: 600px;
  background-color: red;
} */
</style>