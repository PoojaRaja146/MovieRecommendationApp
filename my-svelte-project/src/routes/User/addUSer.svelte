<script>
    import axios from 'axios';
  
    let newUser = { name: '', email: '' };
  
   
    async function addUser() {
    const formData = new URLSearchParams();
    formData.append('name', newUser.name);
    formData.append('email', newUser.email);

    try {
      const response = await axios.post('http://localhost:8086/api/v1/users/add', formData, {
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
  
  <h1>Users</h1>
   
  <h2>Add User</h2>
  <form on:submit|preventDefault={addUser}>
    <label>
      Name:
      <input bind:value={newUser.name} />
    </label>
    <label>
      Email:
      <input bind:value={newUser.email} />
    </label>
    <button type="submit">Add User</button>
  </form>
  