<template>
  <div id="app">
    <div class="container">
      <h1>Random Movie Generator</h1>
      <p>Get a random movie suggestion!</p>

      <!-- Button to generate random movie -->
      <button @click="fetchRandomMovie">Generate Movie</button>

      <!-- Display the generated movie -->
      <MovieCard v-if="movie" :movie="movie" />
      <p v-if="error" class="error-message">No movie found! Try again.</p>
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
      movie: null, // Movie generated
      error: false, // Error state for API
      movieList: [
        "tt0111161", // The Shawshank Redemption
        "tt0068646", // The Godfather
        "tt0071562", // The Godfather: Part II
        "tt0468569", // The Dark Knight
        "tt0050083", // 12 Angry Men
        "tt0108052", // Schindler's List
        "tt0137523", // Fight Club
        "tt1375666", // Inception
        "tt0080684", // Star Wars: Episode IV - A New Hope
        "tt0133093", // The Matrix
        // Add more IDs for other movies here
      ],
    };
  },
  methods: {
    async fetchRandomMovie() {
      const apiKey = process.env.VUE_APP_API_KEY; // API Key
      const apiUrl = "https://www.omdbapi.com/";

      try {
        // Select a random movie ID from the list
        const randomMovieId = this.movieList[Math.floor(Math.random() * this.movieList.length)];

        // Set parameters for API request
        const params = {
          i: randomMovieId, // Selected IMDb ID
          apikey: apiKey,
        };

        // Make API call
        const response = await axios.get(apiUrl, { params });

        if (response.data.Response === "True") {
          this.movie = response.data; // Set the found movie
          this.error = false; // Reset error
        } else {
          this.error = true; // Set error if no movie found
          this.movie = null; // Remove movie if error
        }
      } catch (error) {
        console.error("Error with API call", error);
        this.error = true;
      }
    },
  },
};
</script>

<style scoped>
/* Overall container */
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px;
  text-align: center;
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  border-radius: 10px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 20px;
  text-transform: uppercase;
}

p {
  font-size: 1.2rem;
  margin-bottom: 30px;
  color: #555;
}

button {
  padding: 15px 25px;
  font-size: 1.2rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s ease-in-out;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

button:hover {
  background-color: #0056b3;
  transform: translateY(-4px); /* Small hover effect */
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.error-message {
  color: red;
  font-size: 1.1rem;
  margin-top: 20px;
}

.movie-card {
  animation: fadeIn 1s ease-in-out;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
