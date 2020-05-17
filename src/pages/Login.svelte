
<script>
	import { router } from 'tinro';

	function googleSignIn(){
		var provider = new firebase.auth.GoogleAuthProvider();

		firebase.auth().signInWithPopup(provider).then(function(result) {
            router.goto("/todos")
		}).catch(function(error) {
			console.log(error.message)
		});

	}
</script>


<div class="page">


<h2 class="title has-text-centered" style="margin-top:50px">TODO</h2>

<div class="box" style="margin-top:50px">
<button on:click={googleSignIn} class="button is-dark is-fullwidth">Sign In with Google</button>
</div>

</div>
