<template>
  <div class="hello">
    <audio ref="press0" :src="pressWav" />
    <audio ref="press1" :src="pressWav" />
    <audio ref="press2" :src="pressWav" />
    <audio ref="press3" :src="pressWav" />
    <audio ref="reset" :src="resetWav" />
    <audio ref="won" :src="wonMp3" />
    <table>
      <tbody>
        <tr v-for="row, y in state">
          <td v-for="cell, x in row">
            <light :on="cell" :permanent="shape[y][x]" />
          </td>
        </tr>
      </tbody>
    </table>
    <message-box :stage="presses < 12 ? 0 : 1" @win="win" />
  </div>
</template>

<script>
/* eslint-disable */
import shape from './shape';
import MessageBox from './MessageBox';
import Light from './Circle';
import pressWav from '../assets/press.wav';
import resetWav from '../assets/reset.wav';
import wonMp3 from '../assets/won.mp3';
import shuffle from './shuffle';

export default {
  name: 'HelloWorld',
  components: {
    MessageBox,
    Light,
  },
  methods: {
    win() {
      this.$emit('win');
      this.$refs.won.play();
    },
    press(n1) {
      const n = this.remap[n1];
      // 0 <= n < 26
      const newState = [];
      if (n >= 20) {
        for (let y = 0; y < this.t.length; y += 1) {
          const row = [];
          for (let x = 0; x < this.t[y].length; x += 1) {
            row.push(true);
          }
          newState.push(row);
        }
        this.$refs.reset.play();
        this.$emit('flash');
        this.state = newState;
        return;
      }

      let changed = false;
      for (let y = 0; y < this.t.length; y += 1) {
        const row = [];
        for (let x = 0; x < this.t[y].length; x += 1) {
          const newVal = this.t[y][x] === n ? false : this.state[y][x];
          if (this.state[y][x] !== newVal) {
            changed = true;
          }
          row.push(newVal);
        }
        newState.push(row);
      }
      if (changed) {
        this.presses += 1;
        const ref = this.$refs[`press${this.sfx % 4}`];
        ref.play();
        this.sfx += 1;

        window.setTimeout(() => {
          this.state = newState;
        }, 50);
      }
    },
  },
  data() {
    const w = shape[0].length;
    const h = shape.length;

    const state = [];
    // Label the grid with numbers; it's 5x4 really.
    const w2 = 5;
    const h2 = 4;
    const t = [];
    for (let y = 0; y < h; y += 1) {
      const row = [];
      const stateRow = [];
      for (let x = 0; x < w; x += 1) {
        const y2 = Math.floor((y * h2) / h);
        const x2 = Math.floor((x * w2) / w);
        row.push((y2 * w2) + x2);
        stateRow.push(true);
      }
      t.push(row);
      state.push(stateRow);
    }
    for (let i = 0; i < 800; i += 1) {
      const sx = Math.floor(Math.random() * w);
      const sy = Math.floor(Math.random() * h);
      const dx = Math.min(w - 1, Math.max(0, sx + (Math.round(Math.random() * 4) - 2)));
      const dy = Math.min(h - 1, Math.max(0, sy + (Math.round(Math.random() * 4) - 2)));
      const tmp = t[dy][dx];
      t[dy][dx] = t[sy][sx];
      t[sy][sx] = tmp;
    }
    const remap = [];
    for (let i = 0; i < 26; i += 1) {
      remap.push(i);
    }
    shuffle(remap);
    return {
      shape,
      state,
      t,
      pressWav,
      resetWav,
      wonMp3,
      sfx: 0,
      presses: 0,
      remap,
    };
  },
  mounted() {
    window.addEventListener('keypress', (e) => {
      const string = String.fromCharCode(e.keyCode).toLowerCase();
      if (string.match(/[a-z]/)) {
        this.press(string.charCodeAt(0) - 97);
      }
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  margin: 30px auto;
}
td {
  padding: 7px;
  vertical-align: middle;
  text-align: center;
}

</style>
