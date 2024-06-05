<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let name = [];
    let newGenre = { name: ''}; 
   
  
    async function addUser() {
    const formData = new URLSearchParams();
    formData.append('name', newGenre.name);

    try {
      const response = await axios.post('http://localhost:8086/api/v1/genres/add', formData, {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        }
      });
      console.log(response.data);
    } catch (error) {
      console.error(error);
    }
  }
  </script>
  
  <h2>Add Genre</h2>
  <form on:submit|preventDefault={addUser}>
    <label>
      Name:
      <input bind:value={newGenre.name} />
    </label>
    
    <button type="submit">Add Genre</button>
  </form>
  