<script>
  import { link } from 'svelte-routing';

  let username = '';
  let password = '';
  let errorMessage = '';
  let isAuthenticated = false;

  async function fetchUsers() {
    const response = await fetch('http://localhost:8086/api/v1/users/all');
    const users = await response.json();
    return users;
  }

  async function handleLogin() {
    const users = await fetchUsers();
    const validUsernames = users.map((user) => user.firstName);

    // Validate username
    if (!validUsernames.includes(username)) {
      errorMessage = 'Invalid username';
      return;
    }

    // Replace this with your actual authentication logic
    if (password === 'admin') {
      // Successful login
      isAuthenticated = true;
      alert('Login successful!');
    } else {
      // Invalid credentials
      errorMessage = 'Invalid password';
    }
  }
</script>

<main>
  <h1>Login</h1>
  {#if errorMessage}
    <p class="error">{errorMessage}</p>
  {/if}
  <form on:submit|preventDefault={handleLogin}>
    <label>
      Username:
      <input type="text" bind:value={username} required />
    </label>
    <label>
      Password:
      <input type="password" bind:value={password} required />
    </label>
    <button type="submit">Login</button>
  </form>
  {#if isAuthenticated}
    <a href="/admin" use:link>Go to Admin Page</a>
  {/if}
</main>

<style>
  .error {
    color: red;
  }
</style>