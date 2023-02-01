<template>
  <div>
    <input type="file" @change="loadFile" />
    <AudioPlayer v-if="file" :file="file"></AudioPlayer>
    
  </div>
</template>
<style >
  body {
    background-color: #f3f3f3;
  }
  * {
    box-sizing: border-box;
  }
</style>
<script>
import AudioPlayer from './components/AudioPlayer.vue'

export default {
  components: {
    AudioPlayer
  },
  data() {
    return {
      file: null
    };
  },
  methods: {
    
    loadFile(event) {
      const file = event.target.files[0];
      this.file = file
    }
  }
};
</script>

