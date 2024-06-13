<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let users = [];
    let selectedUserid = null;
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/users/all');
      users = response.data;
    });
  
    async function deleteUser() {
      try {
        const response = await axios.delete(`http://localhost:8086/api/v1/users/delete/${selectedUserid}`);
        console.log(response.data);
        selectedUserid = null;
        const updatedResponse = await axios.get('http://localhost:8086/api/v1/users/all');
        users = updatedResponse.data;
      } catch (error) {
        console.error('An error occurred:', error);
      }
    }
  </script>
  
  <h1>user</h1>
  
  <ul>
    {#each users as user}
      <li>
        {user.name}
        <button on:click={() => selectedUserid = user.id}>Delete</button>
      </li>
    {/each}
  </ul>
  
  {#if selectedUserid}
    <div>
      <p>Are you sure you want to delete the genre with ID {selectedUserid}?</p>
      <button on:click={deleteUser}>Confirm Delete</button>
      <button on:click={() => selectedUserid = null}>Cancel</button>
    </div>
  {/if}