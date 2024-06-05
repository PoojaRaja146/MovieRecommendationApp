<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let genres = [];
    let selectedGenreId = null;
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/genres/all');
      genres = response.data;
    });
  
    async function deleteGenre() {
      try {
        const response = await axios.delete(`http://localhost:8086/api/v1/genres/delete/${selectedGenreId}`);
        console.log(response.data);
        selectedGenreId = null;
        const updatedResponse = await axios.get('http://localhost:8086/api/v1/genres/all');
        genres = updatedResponse.data;
      } catch (error) {
        console.error('An error occurred:', error);
      }
    }
  </script>
  
  <h1>Genres</h1>
  
  <ul>
    {#each genres as genre}
      <li>
        {genre.name}
        <button on:click={() => selectedGenreId = genre.id}>Delete</button>
      </li>
    {/each}
  </ul>
  
  {#if selectedGenreId}
    <div>
      <p>Are you sure you want to delete the genre with ID {selectedGenreId}?</p>
      <button on:click={deleteGenre}>Confirm Delete</button>
      <button on:click={() => selectedGenreId = null}>Cancel</button>
    </div>
  {/if}