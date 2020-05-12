<script>
	export let name;
	export let bold123=false;
</script>

{#if bold123}
<b>
	<p>{name}</p>
</b>
{:else}
	<p>{name}</p>
{/if}
<hr>