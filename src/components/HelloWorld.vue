<template>
  <div class="hello" :class="{ black: win }">
    <div class="hole" v-for="(k, idx) in holes" :key="idx">
      <div class="mole" :class="{ hide: k }">
        <a @click = "hit(idx)" :class="{ hit: hiting }">
          <img src="./mole.png">
        </a>
      </div>
    </div>
    <h2>目前{{score}}分</h2>
    <h2>剩下{{t}}秒</h2>
    <button class="big" v-show="end" @click="end=false; t=60; score=0; win=false">再來一局</button>
    <a id = "win" v-show="win" @click="end = false; t = 60; score = 0; win = false">
      <img src="./win2.png"/>
    </a>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  methods: {
    unhit() {
      this.hiting = false;
    },
    hit(k) {
      this.hiting = true;
      if (!this.end && !this.holes[k]) {
        this.holes[k] = true;
        this.$forceUpdate();
        this.score += 1;
      }
      setTimeout(this.unhit, 50);
    },
    time() {
      if (!this.end) {
        this.t -= 1;
      }
      if (this.t <= 0) {
        this.end = true;
        if (this.score >= 30) {
          this.win = true;
        }
      }
    },
    reset() {
      if (!this.end) {
        this.holes = this.holes.map(() => [true, false][Math.floor(Math.random() * 2)]);
      }
    },
  },
  mounted() {
    this.reset();
    setInterval(this.reset, 3000);
    setInterval(this.time, 1000);
  },
  data() {
    return {
      hiting: false,
      win: false,
      end: false,
      score: 0,
      t: 60,
      holes: [
        false, false, false,
        false, false, false,
        false, false, false,
      ],
    };
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.big {
  font-size: 36px;
  font-weight: bold;
}

.black {
  background-color: black;
  background-image: url('./color.png');
}

a {
  cursor: url('./hammer2.png'), pointer;
}

a.hit {
  cursor: url('./hammer3.png'), pointer;
}

.hole {
  display: inline-block;
  overflow: hidden;
}

.mole {
  width: 28vw;
}

.mole img {
  width: 100%;
  position: relative;
  top: 0;
  transition: all 2s ease;
}

.mole.hide img {
  position: relative;
  top: 300px;
}

#win {
  position: fixed;
  width: 50vw;
  top: 10vh;
  left: 25vw;
}

#win img {
  width: 100%;
}

</style>
