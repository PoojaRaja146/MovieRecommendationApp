<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let users = [];
    let searchTerm = '';
    let filteredUsers = [];
    let selectedUser = null;
    let userDetails = null;
    
    // Fetch all users on component mount
    onMount(async () => {
        try {
            const response = await axios.get('http://localhost:8086/api/v1/users/all');
            users = response.data;
            filteredUsers = users;
        } catch (error) {
            console.error('Error fetching users:', error);
        }
    });
  
    // Reactive statement to filter users based on search term
    $: filteredUsers = users.filter(user =>
        user.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  
    // Function to fetch user details by user ID
    async function fetchUserDetails(userId) {
        try {
            const response = await axios.get(`http://localhost:8086/api/v1/users/${userId}`);
            console.log(response.data); // Log the response to see the structure
            userDetails = response.data; // Assuming response.data contains user details
        } catch (error) {
            console.error('Error fetching user details:', error);
        }
    }
  
    // Function to handle user selection
    function selectUser(user) {
        selectedUser = user;
        fetchUserDetails(user.id);
    }
</script>
  
<main>
    <h1>Search Users</h1>
  
    <input
        type="text"
        placeholder="Search users..."
        bind:value={searchTerm}
    />
  
    <ul>
        {#each filteredUsers as user}
            <li>
                <button on:click={() => selectUser(user)}>{user.name}</button>
            </li>
        {/each}
    </ul>
  
    {#if selectedUser}
        <h2>User Details for {selectedUser.name}</h2>
        {#if selectedUser}
            <ul>
                <li>ID: {selectedUser.id}</li>
                <li>Name: {selectedUser.name}</li>
                <li>Email: {selectedUser.email}</li>
                <!-- Add more details as needed -->
            </ul>
        {:else}
            <p>Loading details...</p>
        {/if}
    {/if}
</main>
