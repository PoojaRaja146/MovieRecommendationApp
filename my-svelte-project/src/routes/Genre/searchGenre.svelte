<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let genres = [];
    let searchTerm = '';
    let filteredGenres = [];
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/genres/all');
      genres = response.data;
      filteredGenres = genres;
    });
  
    $: filteredGenres = genres.filter(genre =>
      genre.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
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
        <li>{genre.name}</li>
      {/each}
    </ul>
  </main>