<template>
  <div id="app">
    <div class="container">
      <h1>Random Movie Generator</h1>
      <p>Get a random movie suggestion!</p>

      <!-- Button to generate random movie -->
      <button @click="fetchRandomMovie">Generate Movie</button>

      <!-- Display the generated movie -->
      <div v-if="movie" class="movie-card-container">
        <MovieCard v-if="movie" :movie="movie" />
      </div>

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
        "The Shawshank Redemption",
        "The Godfather",
        "The Dark Knight",
        "Schindler's List",
        "12 Angry Men",
        "Inception",
        "Forrest Gump",
        "The Matrix",
        "The Lord of the Rings: The Return of the King",
        "Pulp Fiction",
        "Fight Club",
        "The Silence of the Lambs",
        "The Lion King",
        "The Prestige",
        "The Departed",
        "Gladiator",
        "The Green Mile",
        "The Truman Show",
        "The Sixth Sense",
        "The Social Network",
        "The Pursuit of Happyness",
        "The Grand Budapest Hotel",
        "The Intouchables",
        "The Revenant",
        "The Martian",
        "The Wolf of Wall Street",
        "The Big Short",
        "The King's Speech",
        "The Artist",
        "The Shape of Water",
        "The Favourite",
        "The Irishman",
        "The Gentlemen",
        "The Batman",
        "The Matrix Resurrections",
        // Add more movie titles here
      ],
    };
  },
  methods: {
    async fetchRandomMovie() {
      const apiKey = process.env.VUE_APP_API_KEY; // API Key
      const apiUrl = "https://www.omdbapi.com/";

      try {
        // Select a random movie title from the movieList
        const randomMovieTitle = this.movieList[Math.floor(Math.random() * this.movieList.length)];

        // Get IMDb ID from movie title
        const imdbId = await this.getMovieIdFromTitle(randomMovieTitle);

        if (imdbId) {
          const params = {
            apikey: apiKey,
            i: imdbId, // IMDb ID of the movie
          };

          // Make the API request using IMDb ID
          const response = await axios.get(apiUrl, { params });

          if (response.data.Response === "True") {
            this.movie = response.data; // Set the movie data
            this.error = false;
          } else {
            this.error = true;
            this.movie = null;
          }
        } else {
          this.error = true;
          this.movie = null;
        }
      } catch (error) {
        console.error("Error with API call", error);
        this.error = true;
      }
    },

    // Helper function to get IMDb ID from movie title
    async getMovieIdFromTitle(title) {
      const apiKey = process.env.VUE_APP_API_KEY;
      const apiUrl = "https://www.omdbapi.com/";

      const params = {
        apikey: apiKey,
        t: title, // Title parameter
      };

      try {
        const response = await axios.get(apiUrl, { params });
        if (response.data.Response === "True") {
          return response.data.imdbID; // Return IMDb ID if found
        } else {
          console.error("Movie not found for title:", title);
          return null;
        }
      } catch (error) {
        console.error("Error fetching movie ID", error);
        return null;
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
  background-color: #97302c;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s ease-in-out;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

button:hover {
  background-color: #b33a36;
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
</style>
