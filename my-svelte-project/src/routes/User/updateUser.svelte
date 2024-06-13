<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let users = [];
    let editingUser = null;
    let updatedUser = { id: '', firstName: '', lastName: '', email: '', phoneNo:'' };
  
    // Function to handle the form submission for updating a user
    async function updateuser() {
      try {
        console.log(updatedUser.id, updatedUser);
        const response = await axios.put(`http://localhost:8086/api/v1/users/update/${updatedUser.id}`,
        null,
        {
          params: {
            firstName: updatedUser.firstName,
            lastName: updatedUser.lastName,
            email: updatedUser.email,
            phoneNo: updatedUser.phoneNo
          },
          headers: {
            'Content-Type': 'application/json'
          }
        });
  
        console.log(response.data);
        editingUser = null;
        updatedUser = { id: '', firstName: '', lastName: '', email: '', phoneNo:'' };
        const response2 = await axios.get('http://localhost:8086/api/v1/users/all');
        users = response2.data;
      } catch (error) {
        console.error(error);
      }
    }
  
    // Fetch the initial list of users when the component mounts
    onMount(async () => {
      try {
        const response = await axios.get('http://localhost:8086/api/v1/users/all');
        users = response.data;
      } catch (error) {
        console.error(error);
      }
    });
  
    // Function to select a user for editing
    function selectUser(user) {
      editingUser = user;
      updatedUser = { ...user };
    }
  
    // Function to cancel editing
    function cancelEdit() {
      editingUser = null;
      updatedUser = { id: '', firstName: '', lastName: '', email: '' , phoneNo:''};
    }
  </script>
  
  <!-- HTML Form to update user details -->
  <form on:submit|preventDefault={updateuser}>
    <div>
      <label for="id">user ID:</label>
      <input type="number" id="id" bind:value={updatedUser.id} required readonly={editingUser !== null} />
    </div>
    <div>
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" bind:value={updatedUser.firstName} required />
    </div>
    <div>
      <label for="lastName">Last Name:</label>
      <input type="text" id="lastName" bind:value={updatedUser.lastName} required />
    </div>
    <div>
      <label for="email">email:</label>
      <input type="text" id="email" bind:value={updatedUser.email} required />
    </div>
    <button type="submit">{editingUser ? 'Update user' : 'Add user'}</button>
    {#if editingUser}
      <button type="button" on:click={cancelEdit}>Cancel</button>
    {/if}
  </form>
  
  <!-- Display list of users -->
  {#each users as user}
    <div>
      <p>{user.firstName} {user.lastName} ({user.email})</p>
      <button type="button" on:click={() => selectUser(user)}>Edit</button>
    </div>
  {/each}
  