<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let directors = [];
    let searchTerm = '';
    let filteredDirectors = [];
    let selectedDirector = null;
    let movies = [];
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/directors/all');
      directors = response.data;
      filteredDirectors = directors;
    });
  
    $: filteredDirectors = directors.filter(director =>
      director.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  
    async function fetchMovies(directorId) {
      try {
        const response = await axios.get(`http://localhost:8086/api/v1/directors/${directorId}/movies`);
        movies = response.data.split('\n').map(movie => ({ details: movie }));
      } catch (error) {
        console.error('Error fetching movies:', error);
      }
    }
  
    function selectDirector(director) {
      selectedDirector = director;
      fetchMovies(director.id);
    }
  </script>
  
  <main>
    <h1>Search Directors</h1>
  
    <input
      type="text"
      placeholder="Search directors..."
      bind:value={searchTerm}
    />
  
    <ul>
      {#each filteredDirectors as director}
        <li>
          <button on:click={() => selectDirector(director)}>{director.name}</button>
        </li>
      {/each}
    </ul>
  
    {#if selectedDirector}
      <h2>Movies by {selectedDirector.name}</h2>
      <ul>
        {#each movies as movie}
          <li>{movie.details}</li>
        {/each}
      </ul>
    {/if}
  </main>