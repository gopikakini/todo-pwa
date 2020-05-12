<script>

export let todoid;
export let todoname;
export let user;

let items = []
let newitem;

var db = firebase.firestore();

function loadItems(todoid){
		
		db.collection("todo").where("todoid", "==", todoid).onSnapshot(function(querySnapshot){
			items=[];
			querySnapshot.forEach((doc) => {
				console.log(`${doc.id} => ${doc.data()}`);

				let item = {
					"id": doc.id,
					"title":doc.data()["text"]
				}

				items = [...items, item]
			});
		});
	}


loadItems(todoid)


function addItem(){
    db.collection("todo").add({
			text: newitem,
			todoid: todoid
		})
		.then(function(docRef) {
			console.log("Document written with ID: ", docRef.id);
			newitem = ""
		})
		.catch(function(error) {
			console.error("Error adding document: ", error);
		});
}

</script>

<a href="/todos">Back</a>

<h2>{decodeURIComponent(todoname)}</h2>

<a href={"/share/" + todoid + "/" + todoname}>Share</a>

<ul>

{#each items as item}
<li id={item.id}>{item.title}</li>
{/each}

</ul>

<input type="text" bind:value={newitem}>
<button on:click={addItem}>Add Item</button>