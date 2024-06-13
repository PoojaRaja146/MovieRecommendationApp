<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let directors = [];
    let newDirector = { name: '', gender: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/directors/all');
      directors = response.data;
    });
  
    
  </script>
  
  <h1>directors</h1>
  
  <ul>
    {#each directors as user}
      <li>{user.name} ({user.gender})</li>
    {/each}
  </ul>
  
  