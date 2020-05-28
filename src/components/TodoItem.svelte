<script>
  export let id;
  export let title;
  export let checked;

  var db = firebase.firestore();

  function deleteTodo(){
    db.collection("todo").doc(id).delete()
  }

  function checkItem(){
    console.log(checked)
    db.collection("todo").doc(id).update({"checked": checked})
  }

</script>

<div class="card">
  <div class="card-content">
    <div class="columns is-mobile">
      <div class="column is-2"><input type="checkbox" bind:checked={checked} on:change={checkItem}></div>
      
      <div class="column"><p class="subtitle">
      {#if checked}
        <del> {title}</del>
        {:else}
        {title}
      {/if}    
     </p></div>
      <div class="column is-3">
        <button class="button is-dark is-small" on:click={deleteTodo}>X</button>
      </div>
    </div>
    
  </div>
</div>
