<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let users = [];
    let newUser = { firstName: '', lastName:'', email: '' };
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/users/all');
      users = response.data;
    });
  
    
  </script>
  
  <h1>Users</h1>
  
  <ul>
    {#each users as user}
      <li>{user.firstName} {user.lastName} ({user.email})</li>
    {/each}
  </ul>
  
  