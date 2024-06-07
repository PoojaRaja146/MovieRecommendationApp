<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let name = [];
    let newDirector = { name: '', gender: ''}; 
   
  
    async function addDirector() {
    const formData = new URLSearchParams();
    formData.append('name', newDirector.name);
    formData.append('gender', newDirector.gender);

    try {
      const response = await axios.post('http://localhost:8086/api/v1/directors/add', formData, {
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
  
  <h2>Add Director</h2>
  <form on:submit|preventDefault={addDirector}>
    <label>
      Name:
      <input bind:value={newDirector.name} />
    </label>
    <label>
        Gender:
        <input bind:value={newDirector.gender} />
      </label>
    <button type="submit">Add Director</button>
  </form>
  