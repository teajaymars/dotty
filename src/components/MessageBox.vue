<template>
  <div class="msg">
    <span style="transition: opacity 1s;" :style="`opacity: ${1 - (stage / 2)}`">
    Press the letter keys to turn off the lights and reveal the code.<br/>
    Beware of the reset keys!<br/></span>
    &nbsp;
    <span style="transition: opacity 1s;" :style="`opacity: ${stage}`">Worked out the code? Enter the numbers: {{ code }}<span class="flashing-cursor">&#9474;</span></span>
  </div>
</template>

<script>
export default {
  props: ['stage'],
  data() {
    return {
      code: '',
    };
  },
  watch: {
    code() {
      if (this.code.trim() === '247') {
        this.$emit('win');
      }
    },
  },
  mounted() {
    window.addEventListener('keydown', (e) => {
      if (this.stage === 0) {
        return;
      }
      if (e.keyCode === 27 || e.keyCode === 8) {
        this.code = '';
        return;
      }
      const string = String.fromCharCode(e.keyCode).toLowerCase();
      if (string.match(/[0-9]/)) {
        this.code += string;
      }
    });
  },
};
</script>

<style>
.msg {
  position: fixed;
  bottom: 5%;
  left: 10%;
  right: 10%;
  padding: 25px;
  font-size: 30px;
  color: #fff;
  text-shadow: 0px 0px 1px #fff;
  background: rgba(0, 0, 0, 0.6);
  border-radius: 10px;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.5);

  p {
    margin-top: 0px;
    margin-bottom: 0px;
  }
}

@keyframes anim-flash {
  0% { opacity: 0; }
  29% { opacity: 0; }
  40% { opacity: 0.4; }
  100% { opacity: 0.4; }
}
.flashing-cursor {
  animation: anim-flash;
  animation-duration: 500ms;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
}
</style>
