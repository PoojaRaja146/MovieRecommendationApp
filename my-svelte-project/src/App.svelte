<!-- App.svelte -->
<script>
  import Router from "./routes/Router.svelte";
</script>

<main>
  <Router url={location.pathname} />
</main>