<template>
  <div id="app">
    <button id="showLibary" name="showLibrary" value="Library" v-on:click="showLibrary">Library</button> <button id="drawCard" name="drawCard" value="Draw Card" v-on:click="drawCard">Draw Card</button>
  <div id="library">
    <template v-for="item in myLibrary">
      <div :key="item">
        <img :src="item" />
        <div><b>Name:</b>{{ item.replace(/.*\/img\//i,'').replace('.jpeg','').split('-')[0] }}</div>
        <div><b>Rarity:</b>{{ item.replace(/.*\/img\//i,'').replace('.jpeg','').split('-')[1].split('.')[0] }}</div>
      </div>
    </template>
  </div>
  <div id="scratch">
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
        id="currentCard"
        :src="character"
        @load="init()"
      >
      <h3>{{ subline }}</h3>
    </div>
  </vue-scratchable>
  <p>You scratched off {{ percentage }}%.</p>
  </div>
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
        ? `🎉 There is still ${100 - this.percentage}% left for me to be scratched off... 🎉`
        : `💚 Hello, I'm named ${this.name} and I'm ${this.rarity}. 💚`;
    },
  },
  watch: {
    subline: {
      handler: function() {
        var complete = this.percentage >= 60;
        if (complete) {
          var url = document.getElementById("currentCard").src;
          if (!this.myLibrary.includes(url)) {
            this.myLibrary.push(url);
            console.log(this.myLibrary);
            localStorage.setItem("myLibrary", JSON.stringify(this.myLibrary));
          }
        }
      }
    }
  },
  data() {
    var myLibrary = JSON.parse(localStorage.getItem("myLibrary"));
    if (!myLibrary) {
      myLibrary = [];
    }
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
      characters: characters,
      myLibrary: myLibrary,
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
    showLibrary() {
      document.getElementById("showLibary").style.display = "none";
      document.getElementById("drawCard").style.display = "block";
      document.getElementById("library").style.display = "block";
      document.getElementById("scratch").style.display = "none";
    },
    drawCard() {
      var selectedCharacter = Math.floor(Math.random() * this.characters.length);
      var name = this.characters[selectedCharacter].split('-')[0];
      var rarity = this.characters[selectedCharacter].split('-')[1];
      this.selectedCharacter = this.characters[selectedCharacter];
      this.character = require('./assets/'+this.characters[selectedCharacter]+'.jpeg')
      this.name = name;
      this.rarity = rarity;
      document.getElementById("showLibary").style.display = "block";
      document.getElementById("drawCard").style.display = "none";
      document.getElementById("library").style.display = "none";
      document.getElementById("scratch").style.display = "block";      
    }
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

#drawCard, #library {
  display: none;
}

.vue-scratchable-wrapper {
  background-color: white;
}
h1, h2 { margin-top: 0;}


h3 {
  color: #2c3e50;
  text-align: center;
}

a {
  color: #2196f3;
}
</style>
