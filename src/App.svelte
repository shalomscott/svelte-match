<script>
	import Tile from './Tile.svelte';

	function shuffleArray(a) {
		for (let i = a.length - 1; i > 0; --i) {
			const j = Math.floor(Math.random() * (i + 1));
			[a[i], a[j]] = [a[j], a[i]];
		}
	}

	const tiles = [];
	for (let i = 1; i <= 5; ++i) {
		tiles.push(
			{ number: i, isAlive: true, isClicked: false },
			{ number: i, isAlive: true, isClicked: false }
		);
	}
	shuffleArray(tiles);

	let oldClicked = null;
	let newClicked = null;

	const clickTile = index => () => {
		if (!tiles[index].isAlive || tiles[index].isClicked || newClicked !== null)
			return;
		if (oldClicked === null) {
			oldClicked = index;
			tiles[index].isClicked = true;
			return;
		}
		setTileClicked(index);
	};

	const setTileClicked = index => {
		tiles[index].isClicked = true;
		newClicked = index;
		setTimeout(() => {
			if (tiles[index].number === tiles[oldClicked].number) {
				tiles[index].isAlive = false;
				tiles[oldClicked].isAlive = false;
				tiles[index].isClicked = false;
			}
			tiles[oldClicked].isClicked = false;
			oldClicked = newClicked;
			newClicked = null;
		}, 1000);
	};
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

<main>
	{#each tiles as { number, isAlive, isClicked }, index}
		<Tile {number} {isAlive} {isClicked} on:click={clickTile(index)} />
	{/each}
</main>
