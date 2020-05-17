<script>
  import TodoItem from "../components/TodoItem.svelte";

  export let todoid;
  export let todoname;
  export let user;

  let items = [];
  let newitem;

  var db = firebase.firestore();

  function loadItems(todoid) {
    db.collection("todo")
      .where("todoid", "==", todoid)
      .onSnapshot(function(querySnapshot) {
        items = [];
        querySnapshot.forEach(doc => {
          console.log(`${doc.id} => ${doc.data()}`);

          let item = {
            id: doc.id,
            title: doc.data()["text"]
          };

          items = [...items, item];
        });
      });
  }

  loadItems(todoid);

  function addItem() {
    db.collection("todo")
      .add({
        text: newitem,
        todoid: todoid
      })
      .then(function(docRef) {
        console.log("Document written with ID: ", docRef.id);
        newitem = "";
      })
      .catch(function(error) {
        console.error("Error adding document: ", error);
      });
  }
</script>

<div class="page">

  <div class="columns is-mobile" style="margin-top:20px;">
	<div class="column is-2"><a href="/todos" class="button">&lt;</a></div>
    <div class="column">
      <h2 class="subtitle">{decodeURIComponent(todoname)} List</h2>
    </div>
    <div class="column is-3">
      <a href={'/share/' + todoid + '/' + todoname} class="button">Share</a>
    </div>

  </div>
  

  <ul>

    {#each items as item}
      <TodoItem id={item.id} title={item.title} />
    {/each}

  </ul>

<div class="card">
<div class="card-content">
    <input class="input" type="text" placeholder="Save the birds" bind:value={newitem}>
    <br><br>
    <button on:click={addItem} class="button is-dark is-fullwidth">Add Item</button>
</div>
</div>

</div>
