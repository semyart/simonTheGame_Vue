<template>
  <div
    @click="activateSquare"
    class="square"
    :class="[squareColor, { active: active }]"
  >
    <div class="audio">
      <audio v-if="this.color === 'blue'" class="sound">
        <source src="../assets/sound/1.mp3" type="audio/ogg" />
        <source src="../assets/sound/1.ogg" type="audio/ogg" />
      </audio>
      <audio v-else-if="this.color === 'red'" class="sound">
        <source src="../assets/sound/2.mp3" type="audio/ogg" />
        <source src="../assets/sound/2.ogg" type="audio/ogg" />
      </audio>
      <audio v-else-if="this.color === 'yellow'" class="sound">
        <source src="../assets/sound/3.mp3" type="audio/ogg" />
        <source src="../assets/sound/3.ogg" type="audio/ogg" />
      </audio>
      <audio v-else class="sound">
        <source src="../assets/sound/4.mp3" type="audio/ogg" />
        <source src="../assets/sound/4.ogg" type="audio/ogg" />
      </audio>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    color: String,
    timeBetweenActivation: Number,
  },
  data() {
    return {
      active: false,
    };
  },
  computed: {
    squareColor: function () {
      return {
        "square-blue": this.color === "blue",
        "square-red": this.color === "red",
        "square-yellow": this.color === "yellow",
        "square-green": this.color === "green",
      };
    },
  },
  methods: {
    async activateSquare() {
      if (this.active === false) {
        this.active = true;
        this.$emit("getColor", this.color);
        await new Promise((resolve) =>
          setTimeout(resolve, this.timeBetweenActivation)
        );
        this.active = false;
      }
    },
  },
  watch: {
    active() {
      if (this.active === true) {
        this.$el.children[0].children[0].play();
      }
    },
  },
};
</script>

<style>
.square {
  height: 200px;
  width: 200px;
  opacity: 0.45;
}

.square-red {
  background-color: red;
}

.square-blue {
  background-color: blue;
}

.square-yellow {
  background-color: yellow;
}

.square-green {
  background-color: green;
}

.active {
  opacity: 1;
}

.square:hover {
  border: 3px solid black;
}
</style>
