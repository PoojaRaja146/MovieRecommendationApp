<script>
  import { onMount } from 'svelte';
  import { link } from 'svelte-routing';
  import axios from 'axios';
  import '../../styles/styles.css';

  let movieName = '';
  let movieRating = '';
  let movieSummary = '';
  let language = '';
  let genres = [];
  let selectedGenre = '';
  let actors = [];
  let selectedActor = '';
  let directors = [];
  let selectedDirector = '';

  onMount(async () => {
    const genresResponse = await fetch('http://localhost:8086/api/v1/genres/all');
    genres = await genresResponse.json();

    const actorsResponse = await fetch('http://localhost:8086/api/v1/actors/all');
    actors = await actorsResponse.json();

    const directorsResponse = await fetch('http://localhost:8086/api/v1/directors/all');
    directors = await directorsResponse.json();
  });

  const addMovie = async (event) => {
    event.preventDefault();

    const formData = new URLSearchParams();
    formData.append('name', movieName);
    formData.append('language', language);
    formData.append('genreId', selectedGenre);
    formData.append('actorId', selectedActor);
    formData.append('directorId', selectedDirector);
    formData.append('summary', movieSummary);
    formData.append('rating', movieRating);

    try {
      console.log(formData.toString());
      const response = await axios.post('http://localhost:8086/api/v1/movies/add', formData, {
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        }
      });
      console.log(response.data);

      if (response.status === 200) {
        // Reset the input fields
        movieName = '';
        movieRating = '';
        movieSummary = '';
        language = '';
        selectedGenre = '';
        selectedActor = '';
        selectedDirector = '';
      }
    } catch (error) {
      console.error('Error adding movie:', error);
    }
  };
</script>

<main>
  <form on:submit={addMovie}>
    <h1>Add Movie</h1>

    <div>
      <label for="movie-name">Movie Name:</label>
      <input id="movie-name" bind:value={movieName} />
    </div>

    <div>
      <label for="language">Language:</label>
      <select id="language" bind:value={language}>
        <option value="">Select Language</option>
        <option value="en">English</option>
        <option value="fr">French</option>
        <option value="es">Spanish</option>
        <option value="Tn">Tamil</option>
        <!-- Add more language options as needed -->
      </select>
    </div>

    <div>
      <label for="genre">Genre:</label>
      <select id="genre" bind:value={selectedGenre}>
        <option value="">Select Genre</option>
        {#each genres as genre (genre.id)}
          <option value={genre.id}>{genre.name}</option>
        {/each}
        <option value="other">Other</option>
      </select>
      {#if selectedGenre === 'other'}
        <a href="/addGenre" use:link>Add Genre</a>
      {/if}
    </div>

    <div>
      <label for="actor">Actor:</label>
      <select id="actor" bind:value={selectedActor}>
        <option value="">Select Actor</option>
        {#each actors as actor (actor.id)}
          <option value={actor.id}>{actor.name}</option>
        {/each}
        <option value="other">Other</option>
      </select>
      {#if selectedActor === 'other'}
        <a href="/addActor" use:link>Add Actor</a>
      {/if}
    </div>

    <div>
      <label for="director">Director:</label>
      <select id="director" bind:value={selectedDirector}>
        <option value="">Select Director</option>
        {#each directors as director (director.id)}
          <option value={director.id}>{director.name}</option>
        {/each}
        <option value="other">Other</option>
      </select>
      {#if selectedDirector === 'other'}
        <a href="/addDirector" use:link>Add Director</a>
      {/if}
    </div>

    <div>
      <label for="summary">Summary:</label>
      <input id="summaryText" bind:value={movieSummary} />
    </div>

    <div>
      <label for="rating">Movie Rating:</label>
      <input id="movieRating" bind:value={movieRating} />
    </div>

    <button type="submit">
      Add Movie
    </button>
  </form>
</main>