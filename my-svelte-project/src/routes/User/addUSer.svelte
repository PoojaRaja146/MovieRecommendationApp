<script>
    import axios from 'axios';
  
    let newUser = { firstName: '', lastName: '',  email: '' };
  
   
    async function addUser() {
    const formData = new URLSearchParams();
    formData.append('firstName', newUser.firstName);
    formData.append('lastName', newUser.lastName);
    formData.append('email', newUser.email);
    formData.append('phoneNo',newUser.phoneNo)

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
      FirstName:
      <input bind:value={newUser.firstName} />
    </label>
    <label>
      LastName:
      <input bind:value={newUser.lastName} />
    </label>
    <label>
      Email:
      <input bind:value={newUser.email} />
    </label>
    <label>
      PhoneNo:
      <input bind:value={newUser.phoneNo} />
    </label>
    <button type="submit">Add User</button>
  </form>
  