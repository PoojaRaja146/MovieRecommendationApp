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
    import DeleteDirector from "./Director/deleteDirector.svelte";
    import DeleteUser from "./User/deleteUser.svelte";
    import DeleteActor from "./Actor/deleteActor.svelte";
    import DeleteMovie from "./Movie/deleteMovie.svelte";
    import SearchMovie from "./Movie/searchMovie.svelte";
    import SearchUser from "./User/searchUser.svelte";
    import LoginPage from "./Home/loginPage.svelte";
    import ListDirector from "./Director/listDirector.svelte";
    import ListMovie from "./Movie/listMovie.svelte";
    import ListActor from "./Actor/listActor.svelte";
    import UpdateDirector from "./Director/updateDirector.svelte";
    import UpdateActor from "./Actor/updateActor.svelte";
    import UpdateMovie from "./Movie/updateMovie.svelte";
    import UpdateUser from "./User/updateUser.svelte";

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

    <Route path="/deleteDirector" component={DeleteDirector} />

    <Route path="/deleteUser" component={DeleteUser} />

    <Route path="/deleteActor" component={DeleteActor} />

    <Route path="/deleteMovie" component={DeleteMovie} />

    <Route path="/searchMovie" component={SearchMovie} />

    <Route path="/searchUser" component={SearchUser} />

    <Route path="/loginPage" component={LoginPage} />

    <Route path="/listDirector" component={ListDirector} />

    <Route path="/listMovie" component={ListMovie} />

    <Route path="/listActor" component={ListActor} />

    <Route path="/updateDirector" component={UpdateDirector} />

    <Route path="/updateActor" component={UpdateActor} />

    <Route path="/updateMovie" component={UpdateMovie} />

    <Route path="/updateUser" component={UpdateUser} />

</Router>