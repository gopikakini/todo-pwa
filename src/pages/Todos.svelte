<script>

    export let user;

	var db = firebase.firestore();

    let items=[];
    $: items_sorted = items.sort(function(a,b){
        return a.createdAt > b.createdAt
    })
    let todoListName;

    firebase.auth().onAuthStateChanged(function(u) {
        if (u) {
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

</script>


<h1>Todo Lists </h1>

{#each items_sorted as item}
    <li><a href={"/todo/"+item.id+"/"+item.name}>{item.name}</a></li>
    
{/each}

<input type="text" bind:value={todoListName}>
<button on:click={addTodoList}>Create List</button>