<script>
	export let number = NaN;
	export let isAlive = true;
	export let isClicked = false;
</script>

<label>
	<button on:click>{isAlive ? '👼' : '☠️'}</button>
	{isClicked ? number : ''}
</label>
