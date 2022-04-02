<template>
  <div id="app">
 <h1>You've gotten a card.</h1>
  <h2>Scratch to reveal your card.</h2>
  <vue-scratchable
    v-slot="{ init }"
    :brushOptions="brush"
    :hideOptions="hide"
    getPercentageCleared
    @percentage-update="updatePoints"
  >
    <div class="wrapper">
      <img
        :src="require('./assets/boxy-rare.jpeg')"
        @load="init()"
      >
      <h3>{{ subline }}</h3>
    </div>
  </vue-scratchable>
  <p>You scratched {{ percentage }}% free.</p>
</div>
</template>

<script>
import VueScratchable from 'vue-scratchable';

export default {
  name: 'App',
  components: {
    VueScratchable
  },
  computed: {
    subline() {
      return this.percentage < 100
        ? `🎉 There is still ${100 - this.percentage}% left for me to be free... 🎉`
        : '💚 Thank you for scratching me free! 💚';
    },
  },
  data() {
    
    var characters = ['boxy-rare'];
    var selectedCharacter = Math.floor(Math.random() * characters.length);
    
    return {
      percentage: 0,
      selectedCharacter: characters[selectedCharacter],
      character: require('./assets/'+characters[selectedCharacter]+'.jpeg'),
      hide: {
        type: 'pattern',
        src: require('./assets/gold.jpg'),
        repeat: 'repeat',
      },
      brush: {
        size: 60,
        shape: 'round',
      },
    };
  },
  methods: {
    updatePoints(percentage) {
      this.percentage = percentage;
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  margin-top: 50px;
}

.vue-scratchable-wrapper {
  background-color: white;
}

h3 {
  color: #2c3e50;
  text-align: center;
}

a {
  color: #2196f3;
}
</style>
