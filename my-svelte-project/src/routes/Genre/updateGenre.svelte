<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  let movies = [];
  let editingMovie = null;
  let updatedMovie = { id: '', summary: '', rating: '' };

  // Function to handle the form submission for updating movie summary and rating
  async function updateMovie() {
    try {
      const { id, summary, rating } = updatedMovie;
      const response = await axios.put(`http://localhost:8086/api/v1/movies/update/${id}`,
      {
        summary,
        rating
      },
      {
        headers: {
          'Content-Type': 'application/json'
        }
      });

      console.log(response.data);
      editingMovie = null;
      updatedMovie = { id: '', summary: '', rating: '' };
      const response2 = await axios.get('http://localhost:8086/api/v1/movies/all');
      movies = response2.data;
    } catch (error) {
      console.error(error);
    }
  }

  // Fetch the initial list of movies when the component mounts
  onMount(async () => {
    try {
      const response = await axios.get('http://localhost:8086/api/v1/movies/all');
      movies = response.data;
    } catch (error) {
      console.error(error);
    }
  });

  // Function to select a movie for editing
  function selectMovie(movie) {
    editingMovie = movie;
    updatedMovie = { ...movie };
  }

  // Function to cancel editing
  function cancelEdit() {
    editingMovie = null;
    updatedMovie = { id: '', summary: '', rating: '' };
  }
</script>

<!-- HTML Form to update movie summary and rating -->
<form on:submit|preventDefault={updateMovie}>
  <div>
    <label for="id">Movie ID:</label>
    <input type="number" id="id" bind:value={updatedMovie.id} required readonly={editingMovie !== null} />
  </div>
  <!-- Display existing summary and rating -->
  {#if editingMovie}
    <div>
      <label for="currentSummary">Current Summary:</label>
      <textarea id="currentSummary" readonly>{editingMovie.metaInfo.summary}</textarea>
    </div>
    <div>
      <label for="currentRating">Current Rating:</label>
      <input type="text" id="currentRating" readonly value={editingMovie.metaInfo.rating} />
    </div>
  {/if}
  <div>
    <label for="summary">New Summary:</label>
    <textarea id="summary" bind:value={updatedMovie.summary} required></textarea>
  </div>
  <div>
    <label for="rating">New Rating:</label>
    <input type="text" id="rating" bind:value={updatedMovie.rating} required />
  </div>
  <button type="submit">Update Summary and Rating</button>
  {#if editingMovie}
    <button type="button" on:click={cancelEdit}>Cancel</button>
  {/if}
</form>

<!-- Display list of movies -->
{#each movies as movie}
  <div>
    <p>{movie.name} ({movie.language}) - Director: {movie.director.firstName} {movie.director.lastName}, Actor: {movie.actor.firstName} {movie.actor.lastName}, Genre: {movie.genre.name}</p>
    <p>{movie.metaInfo.summary} - Rating: {movie.metaInfo.rating}</p>
    <button type="button" on:click={() => selectMovie(movie)}>Edit</button>
  </div>
{/each}
