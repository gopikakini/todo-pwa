<script>

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


<a href={"/todo/" + todoid + "/" + todoname}>Back</a>

<h2>{decodeURIComponent(todoname)}</h2>

<h4>Shared Users</h4>

<ul>
{#each shared_users as u}
<li>{u}</li>
{/each}
</ul>


<input type="text" bind:value={shareto}>
<button on:click={shareTodo}>Share</button>