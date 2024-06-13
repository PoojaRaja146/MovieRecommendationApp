<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let movies = [];
    let newMovie = { name: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/movies/all');
      movies = response.data;
    });
  
    
  </script>
  
  <h1>movies</h1>
  
  <ul>
    {#each movies as user}
      <li>{user.name}</li>
    {/each}
  </ul>
  
  