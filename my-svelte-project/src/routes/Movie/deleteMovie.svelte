<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
    
    let movies = [];
    let selectedMovieid = null;
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/movies/all');
      movies = response.data;
    });
  
    async function deleteMovie() {
      try {
        const response = await axios.delete(`http://localhost:8086/api/v1/movies/delete/${selectedMovieid}`);
        console.log(response.data);
        selectedMovieid = null;
        const updatedResponse = await axios.get('http://localhost:8086/api/v1/movies/all');
        movies = updatedResponse.data;
      } catch (error) {
        console.error('An error occurred:', error);
      }
    }
  </script>
  
  <h1>Movie</h1>
  
  <ul>
    {#each movies as Movie}
      <li>
        {Movie.name}
        <button on:click={() => selectedMovieid = Movie.id}>Delete</button>
      </li>
    {/each}
  </ul>
  
  {#if selectedMovieid}
    <div>
      <p>Are you sure you want to delete the genre with ID {selectedMovieid}?</p>
      <button on:click={deleteMovie}>Confirm Delete</button>
      <button on:click={() => selectedMovieid = null}>Cancel</button>
    </div>
  {/if}