<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    let movies = [];
    let searchTerm = '';
    let filteredmovies = [];
    let selectedMovie = null;
    let movieDetails = null;
    
    // Fetch all movies on component mount
    onMount(async () => {
        try {
            const response = await axios.get('http://localhost:8086/api/v1/movies/all');
            movies = response.data;
            filteredmovies = movies;
        } catch (error) {
            console.error('Error fetching movies:', error);
        }
    });
  
    // Reactive statement to filter movies based on search term
    $: filteredmovies = movies.filter(movie =>
        movie.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  
    // Function to fetch movie details by movie ID
    async function fetchmovieDetails(movieId) {
        try {
            const response = await axios.get(`http://localhost:8086/api/v1/movies/${movieId}`);
            console.log(response.data); // Log the response to see the structure
            movieDetails = response.data; // Assuming response.data contains movie details
        } catch (error) {
            console.error('Error fetching movie details:', error);
        }
    }
  
    // Function to handle movie selection
    function selectMovie(movie) {
        selectedMovie = movie;
        fetchmovieDetails(movie.id);
    }
</script>
  
<main>
    <h1>Search movies</h1>
  
    <input
        type="text"
        placeholder="Search movies..."
        bind:value={searchTerm}
    />
  
    <ul>
        {#each filteredmovies as movie}
            <li>
                <button on:click={() => selectMovie(movie)}>{movie.name}</button>
            </li>
        {/each}
    </ul>
  
    {#if selectedMovie}
        <h2>movie Details for {selectedMovie.name}</h2>
        {#if selectedMovie}
            <ul>
                <li>ID: {selectedMovie.id}</li>
                <li>Name: {selectedMovie.name}</li>
                <!-- Add more details as needed -->
            </ul>
        {:else}
            <p>Loading details...</p>
        {/if}
    {/if}
</main>
