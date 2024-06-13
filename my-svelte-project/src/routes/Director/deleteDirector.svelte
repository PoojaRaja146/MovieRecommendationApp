<script>
  import { onMount } from 'svelte';
  import axios from 'axios';
  
  let directors = [];
  let selectedDirectorid = null;

  onMount(async () => {
    const response = await axios.get('http://localhost:8086/api/v1/directors/all');
    directors = response.data;
  });

  async function deleteDirector() {
    try {
      const response = await axios.delete(`http://localhost:8086/api/v1/directors/delete/${selectedDirectorid}`);
      console.log(response.data);
      selectedDirectorid = null;
      const updatedResponse = await axios.get('http://localhost:8086/api/v1/directors/all');
      directors = updatedResponse.data;
    } catch (error) {
      console.error('An error occurred:', error);
    }
  }
</script>

<h1>Director</h1>

<ul>
  {#each directors as Director}
    <li>
      {Director.firstName}{Director.lastName}
      <button on:click={() => selectedDirectorid = Director.id}>Delete</button>
    </li>
  {/each}
</ul>

{#if selectedDirectorid}
  <div>
    <p>Are you sure you want to delete the genre with ID {selectedDirectorid}?</p>
    <button on:click={deleteDirector}>Confirm Delete</button>
    <button on:click={() => selectedDirectorid = null}>Cancel</button>
  </div>
{/if}