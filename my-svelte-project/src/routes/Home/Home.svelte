<!-- HomePage.svelte -->
<script>
    import { Link } from "svelte-routing";
    import '../../styles/styles.css';
  </script>
  
  
  
  <div class="container">
    <h1>Movie Recommendation App</h1>
    <nav>
      <h1><Link to="/userPage" $$restProps={{ style: 'a:hover { background-color: #b81d24; }' }}>User</Link></h1>
      <h1><Link to="/loginPage" $$restProps={{ style: 'a:hover { background-color: #b81d24; }' }}>Admin Login</Link></h1>
    </nav>
  </div>