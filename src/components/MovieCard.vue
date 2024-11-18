<template>
    <div class="movie-card">
      <h2>{{ movie.Title }}</h2>
      <img :src="movie.Poster !== 'N/A' ? movie.Poster : '/placeholder.png'" alt="Poster del film">
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
  }
  .movie-card img {
    max-width: 100%;
    border-radius: 10px;
  }
  </style>
  