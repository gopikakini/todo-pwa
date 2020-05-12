
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

<button on:click={googleSignIn}>Sign In with Google</button>