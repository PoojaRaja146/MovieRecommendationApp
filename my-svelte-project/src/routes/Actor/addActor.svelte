<script>
    import axios from 'axios';
  
    let name = [];
    let newActor = { name: '', gender: ''}; 
   
  
    async function addDirector() {
    const formData = new URLSearchParams();
    formData.append('name', newActor.name);
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
      Name:
      <input bind:value={newActor.name} />
    </label>
    <label>
        Gender:
        <input bind:value={newActor.gender} />
      </label>
    <button type="submit">Add Director</button>
  </form>
  