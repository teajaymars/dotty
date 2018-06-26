<template>
  <div id="app" :style="`background-image: url('${tile}');`">
    <Game @flash="flash" @win="won = true" />
    <transition name="flash">
      <div v-show="showFlash" class="flash" />
    </transition>
    <transition name="fade">
      <div v-show="won" class="won" :style="`background-image: url(${wonImage});`" />
    </transition>
  </div>
</template>

<script>
import Vue from 'vue';
import Game from './components/Game';
import tile from './assets/tile.jpg';
import wonImage from './assets/won.jpg';

export default {
  name: 'app',
  components: {
    Game,
  },
  methods: {
    flash() {
      this.showFlash = true;
      Vue.nextTick(() => {
        Vue.nextTick(() => {
          this.showFlash = false;
        });
      });
    },
  },
  data() {
    return {
      tile,
      showFlash: false,
      won: false,
      wonImage,
    };
  },
};
</script>

<style>
#app {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background-size: 400px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  transition: transform 5s ease-in;
}
.flash {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: rgba(255, 255, 100, 0.4);
}
.flash-leave-active {
  transition: opacity .5s
}
.flash-leave-to {
  opacity: 0
}

.won {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: black;
  background-size: cover;
}
.fade-enter-active {
  transition: opacity .5s
}
.fade-enter {
  opacity: 0
}
</style>
