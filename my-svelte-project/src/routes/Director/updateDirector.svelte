<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let directors = [];
    let editingdirector = null;
    let updateddirector = { id: '', firstName: '', lastName: '', gender: '' };
  
    // Function to handle the form submission for updating a director
    async function updateDirector() {
      try {
        console.log(updateddirector.id, updateddirector);
        const response = await axios.put(`http://localhost:8086/api/v1/directors/update/${updateddirector.id}`,
        null,
        {
          params: {
            firstName: updateddirector.firstName,
            lastName: updateddirector.lastName,
            gender: updateddirector.gender
          },
          headers: {
            'Content-Type': 'application/json'
          }
        });
  
        console.log(response.data);
        editingdirector = null;
        updateddirector = { id: '', firstName: '', lastName: '', gender: '' };
        const response2 = await axios.get('http://localhost:8086/api/v1/directors/all');
        directors = response2.data;
      } catch (error) {
        console.error(error);
      }
    }
  
    // Fetch the initial list of directors when the component mounts
    onMount(async () => {
      try {
        const response = await axios.get('http://localhost:8086/api/v1/directors/all');
        directors = response.data;
      } catch (error) {
        console.error(error);
      }
    });
  
    // Function to select a director for editing
    function selectDirector(director) {
      editingdirector = director;
      updateddirector = { ...director };
    }
  
    // Function to cancel editing
    function cancelEdit() {
      editingdirector = null;
      updateddirector = { id: '', firstName: '', lastName: '', gender: '' };
    }
  </script>
  
  <!-- HTML Form to update director details -->
  <form on:submit|preventDefault={updateDirector}>
    <div>
      <label for="id">Director ID:</label>
      <input type="number" id="id" bind:value={updateddirector.id} required readonly={editingdirector !== null} />
    </div>
    <div>
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" bind:value={updateddirector.firstName} required />
    </div>
    <div>
      <label for="lastName">Last Name:</label>
      <input type="text" id="lastName" bind:value={updateddirector.lastName} required />
    </div>
    <div>
      <label for="gender">Gender:</label>
      <input type="text" id="gender" bind:value={updateddirector.gender} required />
    </div>
    <button type="submit">{editingdirector ? 'Update Director' : 'Add Director'}</button>
    {#if editingdirector}
      <button type="button" on:click={cancelEdit}>Cancel</button>
    {/if}
  </form>
  
  <!-- Display list of directors -->
  {#each directors as director}
    <div>
      <p>{director.firstName} {director.lastName} ({director.gender})</p>
      <button type="button" on:click={() => selectDirector(director)}>Edit</button>
    </div>
  {/each}
  