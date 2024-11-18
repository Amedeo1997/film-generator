<template>
  <div id="app">
    <div class="container">
      <h1>Non sai che film guardare?</h1>
      <p>Clicca il pulsante per un consiglio casuale!</p>
      <button @click="fetchRandomMovie">Genera Film</button>
      <MovieCard v-if="movie" :movie="movie" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MovieCard from "./components/MovieCard.vue";

export default {
  name: "App",
  components: {
    MovieCard,
  },
  data() {
    return {
      movie: null, // Film da visualizzare
    };
  },
  methods: {
    async fetchRandomMovie() {
      const apiKey = process.env.VUE_APP_API_KEY; // Legge la chiave dall'ambiente
      const apiUrl = "https://www.omdbapi.com/";

      try {
        const randomId = Math.floor(Math.random() * 2000000) + 1; // Genera un ID casuale
        const response = await axios.get(`${apiUrl}?i=tt${randomId.toString().padStart(7, '0')}&apikey=${apiKey}`);

        if (response.data.Response === "True") {
          this.movie = response.data; // Imposta il film recuperato
        } else {
          console.log("Film non trovato, riprovo...");
          this.fetchRandomMovie(); // Riprova con un nuovo ID
        }
      } catch (error) {
        console.error("Errore nella chiamata API", error);
      }
    },
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f3f3f3;
  margin: 0;
}

.container {
  text-align: center;
  margin: 50px auto;
  padding: 20px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 600px;
}
button {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background: #0056b3;
}
</style>
