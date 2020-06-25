<template>
  <v-app>
    <v-app-bar app color="primary" dark>Capital</v-app-bar>

    <v-content>
      <v-card>
        <v-card-title>What's the capital?</v-card-title>
        <v-card-text>
          <h2>{{currentCountry["name"]}}</h2>
          <v-text-field v-model="guess"></v-text-field>
          <v-btn @click="submitGuess">Submit answer</v-btn>
          <p>{{previousAnswerMessage}}</p>
          <h3>Current score:</h3>
          <p>{{score}} / {{attempts}}</p>
        </v-card-text>
      </v-card>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    countries: {},
    currentCountry: {},
    guess: "",
    score: 0,
    attempts: 0,
    previousAnswerMessage: ""
  }),

  mounted() {
    fetch("https://restcountries.eu/rest/v2/all")
      .then(response => {
        return response.json();
      })
      .then(data => {
        this.countries = data;
        this.randomCountryCapital();
      });
  },

  methods: {
    randomCountryCapital() {
      let randomIndex = Math.floor(Math.random() * this.countries.length);
      let randomCountry = this.countries[randomIndex];
      this.countries.splice(randomIndex, 1); // Remove the country
      this.currentCountry = {
        name: randomCountry["name"],
        capital: randomCountry["capital"]
      };
    },
    submitGuess() {
      this.attempts += 1;
      let simpleGuess = this.guess.toLowerCase().trim();
      let simpleCapital = this.currentCountry["capital"].toLowerCase().trim();
      console.log(simpleGuess);
      console.log(simpleCapital);
      let isCorrect = simpleGuess == simpleCapital;
      if (isCorrect) {
        this.score += 1;
        this.previousAnswerMessage = "Correct!";
      } else {
        this.previousAnswerMessage = "Incorrect. ";
        if (this.guess) {
          this.previousAnswerMessage += "You answered: " + this.guess + ". ";
        }
        this.previousAnswerMessage +=
          "The correct answer was: " + this.currentCountry["capital"] + ". ";
      }
      this.guess = "";
      this.randomCountryCapital();
    }
  }
};
</script>
