<template>
    <div class="movie-card">
      <h2>{{ movie.Title }}</h2>
      <img :src="movie.Poster !== 'N/A' ? movie.Poster : '/placeholder.png'" alt="Poster del film" />
      <p><strong>Year:</strong> {{ movie.Year }}</p>
      <p>{{ movie.Plot }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      movie: Object,
    },
  };
  </script>
  
  <style scoped>
  .movie-card {
    text-align: center;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    margin-top: 30px;
  }
  
  .movie-card img {
    max-width: 100%;
    border-radius: 10px;
  }

  h2 {
    font-size: 1.8rem;
    color: #333;
    margin-bottom: 10px;
  }

    p {
        font-size: 1.2rem;
        color: #555;
        margin-bottom: 10px;
    }

    img {
        max-width: 100%;
        border-radius: 10px;
    }
  </style>
  