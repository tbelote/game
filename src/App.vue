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
        :src="character"
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
      return this.percentage < 60
        ? `🎉 There is still ${100 - this.percentage}% left for me to be free... 🎉`
        : `💚 Hello, I'm named ${this.name} and I'm ${this.rarity}. 💚`;
    },
  },
  data() {
    
    var characters = ['amy-rare',
    'bow-common',
    'boxy-rare',
    'charles-common',
    'hello-common',
    'magentaflower-rare',
    'melody-rare',
    'snowdrop-legendary'];
    var selectedCharacter = Math.floor(Math.random() * characters.length);
    var name = characters[selectedCharacter].split('-')[0];
    var rarity = characters[selectedCharacter].split('-')[1];
    
    return {
      percentage: 0,
      selectedCharacter: characters[selectedCharacter],
      name:name,
      rarity:rarity,
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
