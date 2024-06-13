<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let directors = [];
    let selectedDirectorId = null;
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/directors/all');
      directors = response.data;
    });
  
    async function deleteDirector() {
      try {
        const response = await axios.delete(`http://localhost:8086/api/v1/directors/delete/${selectedDirectorId}`);
        console.log(response.data);
        selectedDirectorId = null;
        const updatedResponse = await axios.get('http://localhost:8086/api/v1/directors/all');
        directors = updatedResponse.data;
      } catch (error) {
        console.error('An error occurred:', error);
      }
    }
  </script>
  
  <h1>Director</h1>
  
  <ul>
    {#each directors as director}
      <li>
        {director.name}
        <button on:click={() => selectedDirectorId = director.id}>Delete</button>
      </li>
    {/each}
  </ul>
  
  {#if selectedDirectorId}
    <div>
      <p>Are you sure you want to delete the genre with ID {selectedDirectorId}?</p>
      <button on:click={deleteDirector}>Confirm Delete</button>
      <button on:click={() => selectedDirectorId = null}>Cancel</button>
    </div>
  {/if}