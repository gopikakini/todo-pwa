<script>

export let name 
export let url
export let id

var db = firebase.firestore();

function deleteTodoList(){
  db.collection("todo")
      .where("todoid", "==", id)
      .onSnapshot(function(querySnapshot) {
        querySnapshot.forEach(doc => {
          console.log(`delete ${doc.id} => ${doc.data()}`);
          db.collection("todo").doc(doc.id).delete()
        });
      });
  db.collection("todos").doc(id).delete()
}

</script>


<div class="card">
  <div class="card-content">

  <div class="columns is-mobile">
    <div class="column">
    <p class="subtitle">
            <a href={url}>{name}</a>
        </p>
    </div>
    <div class="column is-3">
      <button class="button is-dark is-small" on:click={deleteTodoList}>X</button>
    </div>
  </div>
  </div>
</div>