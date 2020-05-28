<script>

    import TodoListItem from '../components/TodoListItem.svelte'
    import {router} from 'tinro'; 

    export let user;

	var db = firebase.firestore();

    let items=[];
    $: items_sorted = items.sort(function(a,b){
        return a.createdAt > b.createdAt
    })
    let todoListName;

    firebase.auth().onAuthStateChanged(function(u) {
        if (u) {
            user=u
            getTodoLists()
        }
    })

    function getTodoLists(){
        db.collection("todos").where('users', 'array-contains',user.email).onSnapshot(function(querySnapshot){
			items=[];
			querySnapshot.forEach((doc) => {
				console.log(`${doc.id} => ${doc.data()}`);

				let item = {
					"id": doc.id,
					"name":doc.data()["name"]
				}

                items = [...items, item]
			});
		});
    }

    function addTodoList(){
        db.collection("todos").add({
			name: todoListName,
            "users": [user.email],
            "createdAt":Date.now()
		})
		.then(function(docRef) {
			console.log("Document written with ID: ", docRef.id);
			todoListName = ""
		})
		.catch(function(error) {
			console.error("Error adding document: ", error);
		});
    }

    function signOut(){
		firebase.auth().signOut().then(function() {
			router.goto("/login")
		}).catch(function(error) {
			console.log(error.message)
		});
	}

</script>

<div class="page">

<div class="columns is-mobile" style="margin-top:20px;">

<div class="column">
    <h2 class="title">Todo Lists</h2>
</div>
<div class="column is-4">
    <button class="button is-dark" on:click={signOut}>Sign Out</button>
</div>

</div>

{#each items_sorted as item}

    <TodoListItem url={"/todo/"+item.id+"/"+item.name} name={item.name} id={item.id}/>
    
{/each}

<div class="card">
<div class="card-content">
    <input class="input" type="text" placeholder="Todo List 1" bind:value={todoListName}>
    <br><br>
    <button on:click={addTodoList} class="button is-dark is-fullwidth">Create List</button>
</div>
</div>



</div>