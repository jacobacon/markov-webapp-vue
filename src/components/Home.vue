<template>
  <v-container>
    <v-layout row justify-center>
      <v-card class="mb-4">
        <v-container>
          <h1>Markov Generator</h1>
        </v-container>
      </v-card>
    </v-layout>
    <v-card hover>
      <v-container>
        <v-card-title primary-title>
          <v-layout row justify-center>
            <h3>Training Text</h3>
          </v-layout>
        </v-card-title>
        <v-textarea
          v-model="training"
          placeholder="Enter Training Text"
        ></v-textarea>
        <span style="color: red" v-if="!training"
          >Enter Some Training Text!</span
        >
      </v-container>
    </v-card>
    <v-card class="mt-3" hover>
      <v-container>
        <v-layout column>
          <h3>Generated Text</h3>
          <br />
          <output>{{ generatedText }}</output>
        </v-layout>
      </v-container>
    </v-card>
    <v-card class="mt-3" hover>
      <v-container>
        <v-layout row justify-space-between>
          <span>
            <v-label>Seed Word</v-label>
            <v-text-field
              placeholder="Enter Word"
              v-model="seed"
            ></v-text-field>
          </span>
          <span>
            <v-label for="text-length">Text Length</v-label>
            <v-text-field
              placeholder="value"
              v-model="length"
              type="number"
              min="0"
              max="999"
              id="text-length"
            ></v-text-field>
          </span>
          <v-btn color="green" rounded @click="train()">Generate Text</v-btn>
        </v-layout>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
import Chance from "chance";

let chance = new Chance();
export default {
  name: "Home",
  data() {
    return {
      training: null,
      wordArray: null,
      wordWeights: {},
      generatedText: null,
      seed: null,
      length: 150,
    };
  },
  methods: {
    train: function() {
      if (this.training !== null) {
        this.wordWeights = {};
        this.wordArray = this.training.split(/(?=[ ,.!?\n])/);
        this.wordArray.forEach((word, i) => {
          let nextWord = this.wordArray[++i] || "";
          if (!(word in this.wordWeights)) {
            this.wordWeights[word] = {};
            this.wordWeights[word][nextWord] = 1;
          } else {
            let count = this.wordWeights[word][nextWord];
            this.wordWeights[word][nextWord] = ++count || 1;
          }
        });
        this.generatedText = this.generateText(this.length);
      } else {
        alert("Nothing entered");
      }
    },
    generateText: function(length) {
      if (!this.seed) {
        this.seed = this.wordArray[
          Math.floor(Math.random() * this.wordArray.length)
        ];
      }

      let returnText = this.seed;
      let lastWord = this.seed;

      for (let i = 0; i < length; i++) {
        lastWord = this.getNextWord(lastWord);
        returnText += ` ${lastWord}`;
      }
      return returnText;
    },
    getNextWord: function(word) {
      let nextWordsObject = this.wordWeights[word];

      let nextWords = Object.keys(nextWordsObject);
      let nextWordsWeights = Object.values(nextWordsObject);
      return chance.weighted(nextWords, nextWordsWeights) || "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
