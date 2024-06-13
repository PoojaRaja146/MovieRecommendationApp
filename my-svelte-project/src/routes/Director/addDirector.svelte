<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let firstName = [];
    let newDirector = { firstName: '',lastName: '', gender: ''}; 
   
  
    async function addDirector() {
    const formData = new URLSearchParams();
    formData.append('firstName', newDirector.firstName);
    formData.append('lastName', newDirector.lastName);
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
      FirstName:
      <input bind:value={newDirector.firstName} />
    </label>
    <label>
      LastName:
      <input bind:value={newDirector.lastName} />
    </label>
    <label>
        Gender:
        <input bind:value={newDirector.gender} />
      </label>
    <button type="submit">Add Director</button>
  </form>
  