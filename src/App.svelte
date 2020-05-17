<script>
    import {Route, router} from 'tinro'; 
    router.useHashNavigation();

    import Todos from './pages/Todos.svelte'
    import Todo from './pages/Todo.svelte'
    import ShareTodo from './pages/ShareTodo.svelte'
    import Login from './pages/Login.svelte'


    let user

    firebase.auth().onAuthStateChanged(function(u) {
        if (u) {
            user = u
            router.goto("/todos")

        } else {
            router.goto("/login")
        }
	});

</script>

<Route path="/todos">
    <Todos {user} />
</Route>

<Route path="/todo/:id/:name" let:params>
    <Todo {user} todoid={params.id} todoname={params.name}/>
</Route>

<Route path="/share/:id/:name" let:params>
    <ShareTodo {user} todoid={params.id} todoname={params.name}/>
</Route>

<Route path="/login">
    <Login/>
</Route>