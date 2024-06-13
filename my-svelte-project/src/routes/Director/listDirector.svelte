<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let directors = [];
    let newDirector = { firstName: '',lastName: '', gender: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/directors/all');
      directors = response.data;
    });
  
    
  </script>
  
  <h1>directors</h1>
  
  <ul>
    {#each directors as user}
      <li>{user.firstName} {user.lastName} ({user.gender})</li>
    {/each}
  </ul>
  
  