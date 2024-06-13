<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
    
    let actors = [];
    let selectedActorid = null;
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/actors/all');
      actors = response.data;
    });
  
    async function deleteActor() {
      try {
        const response = await axios.delete(`http://localhost:8086/api/v1/actors/delete/${selectedActorid}`);
        console.log(response.data);
        selectedActorid = null;
        const updatedResponse = await axios.get('http://localhost:8086/api/v1/actors/all');
        actors = updatedResponse.data;
      } catch (error) {
        console.error('An error occurred:', error);
      }
    }
  </script>
  
  <h1>Actor</h1>
  
  <ul>
    {#each actors as Actor}
      <li>
        {Actor.name}
        <button on:click={() => selectedActorid = Actor.id}>Delete</button>
      </li>
    {/each}
  </ul>
  
  {#if selectedActorid}
    <div>
      <p>Are you sure you want to delete the genre with ID {selectedActorid}?</p>
      <button on:click={deleteActor}>Confirm Delete</button>
      <button on:click={() => selectedActorid = null}>Cancel</button>
    </div>
  {/if}