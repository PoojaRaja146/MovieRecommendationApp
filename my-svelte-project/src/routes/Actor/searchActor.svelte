<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let actors = [];
    let searchTerm = '';
    let filteredActors = [];
    let selectedActor = null;
    let movies = [];
  
    onMount(async () => {
      const response = await axios.get('http://localhost:8086/api/v1/actors/all');
      actors = response.data;
      filteredActors = actors;
    });
  
    $: filteredActors = actors.filter(actor =>
      actor.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  
    async function fetchMovies(actorId) {
      try {
        const response = await axios.get(`http://localhost:8086/api/v1/actors/${actorId}/movies`);
        movies = response.data.split('\n').map(movie => ({ details: movie }));
      } catch (error) {
        console.error('Error fetching movies:', error);
      }
    }
  
    function selectActor(actor) {
      selectedActor = actor;
      fetchMovies(actor.id);
    }
  </script>
  
  <main>
    <h1>Search Actors</h1>
  
    <input
      type="text"
      placeholder="Search actors..."
      bind:value={searchTerm}
    />
  
    <ul>
      {#each filteredActors as actor}
        <li>
          <button on:click={() => selectActor(actor)}>{actor.name}</button>
        </li>
      {/each}
    </ul>
  
    {#if selectedActor}
      <h2>Movies with {selectedActor.name}</h2>
      <ul>
        {#each movies as movie}
          <li>{movie.details}</li>
        {/each}
      </ul>
    {/if}
  </main>