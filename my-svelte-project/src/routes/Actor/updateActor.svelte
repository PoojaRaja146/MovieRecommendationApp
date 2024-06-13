<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let actors = [];
    let editingActor = null;
    let updatedActor = { id: '', firstName: '', lastName: '', gender: '' };
  
    // Function to handle the form submission for updating a Actor
    async function updateActor() {
      try {
        console.log(updatedActor.id, updatedActor);
        const response = await axios.put(`http://localhost:8086/api/v1/actors/update/${updatedActor.id}`,
        null,
        {
          params: {
            firstName: updatedActor.firstName,
            lastName: updatedActor.lastName,
            gender: updatedActor.gender
          },
          headers: {
            'Content-Type': 'application/json'
          }
        });
  
        console.log(response.data);
        editingActor = null;
        updatedActor = { id: '', firstName: '', lastName: '', gender: '' };
        const response2 = await axios.get('http://localhost:8086/api/v1/actors/all');
        actors = response2.data;
      } catch (error) {
        console.error(error);
      }
    }
  
    // Fetch the initial list of actors when the component mounts
    onMount(async () => {
      try {
        const response = await axios.get('http://localhost:8086/api/v1/actors/all');
        actors = response.data;
      } catch (error) {
        console.error(error);
      }
    });
  
    // Function to select a Actor for editing
    function selectActor(Actor) {
      editingActor = Actor;
      updatedActor = { ...Actor };
    }
  
    // Function to cancel editing
    function cancelEdit() {
      editingActor = null;
      updatedActor = { id: '', firstName: '', lastName: '', gender: '' };
    }
  </script>
  
  <!-- HTML Form to update Actor details -->
  <form on:submit|preventDefault={updateActor}>
    <div>
      <label for="id">Actor ID:</label>
      <input type="number" id="id" bind:value={updatedActor.id} required readonly={editingActor !== null} />
    </div>
    <div>
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" bind:value={updatedActor.firstName} required />
    </div>
    <div>
      <label for="lastName">Last Name:</label>
      <input type="text" id="lastName" bind:value={updatedActor.lastName} required />
    </div>
    <div>
      <label for="gender">Gender:</label>
      <input type="text" id="gender" bind:value={updatedActor.gender} required />
    </div>
    <button type="submit">{editingActor ? 'Update Actor' : 'Add Actor'}</button>
    {#if editingActor}
      <button type="button" on:click={cancelEdit}>Cancel</button>
    {/if}
  </form>
  
  <!-- Display list of actors -->
  {#each actors as Actor}
    <div>
      <p>{Actor.firstName} {Actor.lastName} ({Actor.gender})</p>
      <button type="button" on:click={() => selectActor(Actor)}>Edit</button>
    </div>
  {/each}
  