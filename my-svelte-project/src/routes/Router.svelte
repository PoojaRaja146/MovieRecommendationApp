<script>

    import { Router, Route } from "svelte-routing";
    import Home from "./Home/Home.svelte";
    import User from "./Home/Users.svelte";
    import AdminPage from "./Home/AdminPage.svelte";
    import Genre from "./Genre/Genre.svelte";
    import Director from "./Admin/Director.svelte";
    import Actor from "./Admin/Actor.svelte";
    import AddGenre from "./Genre/addGenre.svelte";
    import UpdateGenre from "./Genre/updateGenre.svelte";
    import DeleteGenre from "./Genre/deleteGenre.svelte";
    import SearchGenre from "./Genre/searchGenre.svelte";
    

  export let url= "";
</script>

<Router {url}> 
    <Route path="/" component={Home} />

    <Route path="/user" component={User} />
    
    <Route path="/admin" component={AdminPage} />

    <Route path="/genre" component={Genre} />

    <Route path="/director" component={Director} />

    <Route path="/actor" component={Actor} />

    <Route path="/addGenre" component={AddGenre} />

    <Route path="/updateGenre" component={UpdateGenre} />

    <Route path="/deleteGenre" component={DeleteGenre} />

    <Route path="/searchGenre" component={SearchGenre} />

</Router>