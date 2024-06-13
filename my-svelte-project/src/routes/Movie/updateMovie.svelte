<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let movies = [];
    let editingMovie = null;
    let updatedMovie = { id: '', name: '', language: '', directorId: '', actorId: '', genreId: '', summary: '', rating: '' };
  
    // Function to handle the form submission for updating a movie
    async function updateMovie() {
      try {
        console.log(updatedMovie.id, updatedMovie);
        const response = await axios.put(`http://localhost:8086/api/v1/movies/update/${updatedMovie.id}`,
        null,
        {
          params: {
            name: updatedMovie.name,
            language: updatedMovie.language,
            directorId: updatedMovie.directorId,
            actorId: updatedMovie.actorId,
            genreId: updatedMovie.genreId,
            summary: updatedMovie.summary,
            rating: updatedMovie.rating
          },
          headers: {
            'Content-Type': 'application/json'
          }
        });
  
        console.log(response.data);
        editingMovie = null;
        updatedMovie = { id: '', name: '', language: '', directorId: '', actorId: '', genreId: '', summary: '', rating: '' };
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
      updatedMovie = { ...movie, directorId: movie.director.id, actorId: movie.actor.id, genreId: movie.genre.id };
    }
  
    // Function to cancel editing
    function cancelEdit() {
      editingMovie = null;
      updatedMovie = { id: '', name: '', language: '', directorId: '', actorId: '', genreId: '', summary: '', rating: '' };
    }
  </script>
  
  <!-- HTML Form to update movie details -->
  <form on:submit|preventDefault={updateMovie}>
    <div>
      <label for="id">Movie ID:</label>
      <input type="number" id="id" bind:value={updatedMovie.id} required readonly={editingMovie !== null} />
    </div>
    <div>
      <label for="name">Name:</label>
      <input type="text" id="name" bind:value={updatedMovie.name} required />
    </div>
    <div>
      <label for="language">Language:</label>
      <input type="text" id="language" bind:value={updatedMovie.language} required />
    </div>
    <div>
      <label for="directorId">Director ID:</label>
      <input type="number" id="directorId" bind:value={updatedMovie.directorId} required />
    </div>
    <div>
      <label for="actorId">Actor ID:</label>
      <input type="number" id="actorId" bind:value={updatedMovie.actorId} required />
    </div>
    <div>
      <label for="genreId">Genre ID:</label>
      <input type="number" id="genreId" bind:value={updatedMovie.genreId} required />
    </div>
    <div>
      <label for="summary">Summary:</label>
      <textarea id="summary" bind:value={updatedMovie.summary} required></textarea>
    </div>
    <div>
      <label for="rating">Rating:</label>
      <input type="text" id="rating" bind:value={updatedMovie.rating} required />
    </div>
    <button type="submit">{editingMovie ? 'Update Movie' : 'Add Movie'}</button>
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
  