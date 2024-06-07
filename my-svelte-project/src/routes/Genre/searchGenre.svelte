<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  let genres = [];
  let searchTerm = '';
  let filteredGenres = [];
  let selectedGenre = null;
  let movies = [];

  onMount(async () => {
    const response = await axios.get('http://localhost:8086/api/v1/genres/all');
    genres = response.data;
    filteredGenres = genres;
  });

  $: filteredGenres = genres.filter(genre =>
    genre.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  async function fetchMovies(genreId) {
    try {
      const response = await axios.get(`http://localhost:8086/api/v1/genres/${genreId}/movies`);
      movies = response.data.split('\n').map(movie => ({ details: movie }));
    } catch (error) {
      console.error('Error fetching movies:', error);
    }
  }

  function selectGenre(genre) {
    selectedGenre = genre;
    fetchMovies(genre.id);
  }
</script>

<main>
  <h1>Search Genres</h1>

  <input
    type="text"
    placeholder="Search genres..."
    bind:value={searchTerm}
  />

  <ul>
    {#each filteredGenres as genre}
      <li>
        <button on:click={() => selectGenre(genre)}>{genre.name}</button>
      </li>
    {/each}
  </ul>

  {#if selectedGenre}
    <h2>Movies in {selectedGenre.name}</h2>
    <ul>
      {#each movies as movie}
        <li>{movie.details}</li>
      {/each}
    </ul>
  {/if}
</main>