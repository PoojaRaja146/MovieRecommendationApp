<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let actors = [];
    let newActor = { name: '', gender: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/actors/all');
      actors = response.data;
    });
  
    
  </script>
  
  <h1>actors</h1>
  
  <ul>
    {#each actors as user}
      <li>{user.name} ({user.gender})</li>
    {/each}
  </ul>
  
  