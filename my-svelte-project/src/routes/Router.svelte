<script>

    import { Router, Route } from "svelte-routing";
    import Home from "./Home/Home.svelte";
    import User from "./Admin/Users.svelte";
    import AdminPage from "./Home/AdminPage.svelte";
    import Genre from "./Admin/Genre.svelte";
    import Director from "./Admin/Director.svelte";
    import Actor from "./Admin/Actor.svelte";
    import AddGenre from "./Genre/addGenre.svelte";
    import UpdateGenre from "./Genre/updateGenre.svelte";
    import DeleteGenre from "./Genre/deleteGenre.svelte";
    import SearchGenre from "./Genre/searchGenre.svelte";
    import Movie from "./Admin/Movie.svelte";
    import AddMovie from "./Movie/addMovie.svelte";
    import AddDirector from "./Director/addDirector.svelte";
    import AddActor from "./Actor/addActor.svelte";
    import AddUSer from "./User/addUSer.svelte";
    import ListUser from "./User/listUser.svelte";
    import UserPage from "./Home/UserPage.svelte";
    import SearchActor from "./Actor/searchActor.svelte";
    import SearchDirector from "./Director/searchDirector.svelte";
    

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

    <Route path="/movie" component={Movie} />

    <Route path="/addMovie" component={AddMovie} />

    <Route path="/addDirector" component={AddDirector} />

    <Route path="/addActor" component={AddActor} />

    <Route path="/addUser" component={AddUSer} />

    <Route path="/listUser" component={ListUser} />

    <Route path="/userPage" component={UserPage} />

    <Route path="/searchActor" component={SearchActor} />

    <Route path="/searchDirector" component={SearchDirector} />

</Router>