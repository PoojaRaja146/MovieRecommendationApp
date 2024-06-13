<script>
    import axios from 'axios';
  
    let firstName = [];
    let lastName = [];
    let newActor = { firstName: '', lastName:'', gender: ''}; 
   
  
    async function addDirector() {
    const formData = new URLSearchParams();
    formData.append('firstName', newActor.firstName);
    formData.append('lastName', newActor.lastName);
    formData.append('gender', newActor.gender);

    try {
      const response = await axios.post('http://localhost:8086/api/v1/actors/add', formData, {
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
  
  <h2>Add Actor</h2>
  <form on:submit|preventDefault={addDirector}>
    <label>
      FirstName:
      <input bind:value={newActor.firstName} />
    </label>
    <label>
      LastName:
      <input bind:value={newActor.lastName} />
    </label>
    <label>
        Gender:
        <input bind:value={newActor.gender} />
      </label>
    <button type="submit">Add Actor</button>
  </form>
  