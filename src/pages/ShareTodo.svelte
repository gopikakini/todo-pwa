<script>
	import ShareTodo from './ShareTodo.svelte';

export let todoid;
export let todoname;
export let user;


let shareto;

let shared_users = []

var db = firebase.firestore();



function getUsers(){
    db.collection("todos").doc(todoid).get().then(function(doc) {
        if (doc.exists) {
            console.log("Document data:", doc.data());
            shared_users = doc.data().users;
        } else {
            // doc.data() will be undefined in this case
            console.log("No such document!");
        }
    })
}
getUsers()


function shareTodo(){

    shared_users = [...shared_users, shareto]

    db.collection("todos").doc(todoid).update({
        users: shared_users
    })

    shareto=""
}

</script>


<div class="page">

  <div class="columns is-mobile" style="margin-top:20px;">
	<div class="column is-2"><a href={"/todo/" + todoid + "/" + todoname} class="button">&lt;</a></div>
    <div class="column">
      <h2 class="subtitle">{decodeURIComponent(todoname)} List</h2>
    </div>

  </div>

<b>Shared Users</b><br><br>

<ul>
{#each shared_users as u}
    <div class="card">
        <div class="card-content">
        {u}
        </div>
    </div>

{/each}
</ul>


<div class="card">
<div class="card-content">
    <input class="input" type="text" placeholder="john@doe" bind:value={shareto}>
    <br><br>
    <button on:click={shareTodo} class="button is-dark is-fullwidth">Share</button>
</div>
</div>

</div>
