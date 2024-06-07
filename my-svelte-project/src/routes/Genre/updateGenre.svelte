<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let genres = [];
    let editingGenre = null;
    let updatedGenre = { id: '', name: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/genres/all');
      genres = response.data;
    });
  
    function startEditing(genre) {
      editingGenre = genre;
      updatedGenre = { ...genre };
    }
  
    async function updateGenre() {
      try {
        console.log(updateGenre.id,updateGenre);
        const response = await axios.put(`http://localhost:8086/api/v1/genres/update/${updatedGenre.id}?name=${updatedGenre.name}`, {
          headers: {
            'Content-Type': 'application/json'
          }
        });
        console.log(response.data);
        editingGenre = null;
        updatedGenre = { id: '', name: '' };
        const response2 = await axios.get('http://localhost:8086/api/v1/genres/all');
        genres = response2.data;
      } catch (error) {
        console.error(error);
      }
    }
  </script>
  
  <h1>Genres</h1>
  
  <ul>
    {#each genres as genre}
      <li>
        {genre.name}
        <button on:click={() => startEditing(genre)}>Edit</button>
      </li>
    {/each}
  </ul>
  
  {#if editingGenre}
    <h2>Update Genre</h2>
    <form on:submit|preventDefault={updateGenre}>
      <label>
        Genre Name:
        <input bind:value={updatedGenre.name} />
      </label>
      <button type="submit">Update Genre</button>
    </form>
  {/if}