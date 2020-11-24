<script>

	let search;
	let loading = false;
	const API_URL = `https://api.giphy.com/v1/gifs/search?api_key=${API_KEY}&rating=pg&q=`;

	let gifs = [];


	async function formSubmitted(event) {
		event.preventDefault();
		loading = true;
		gifs = []; //reset array on new search
		const url = `${API_URL}${search}`;
		const response = await fetch(url);
		const json = await response.json();
		gifs = json.data.map(gif => gif.images.fixed_height.url);
		loading = false;
	}
</script>

<form action="" on:submit={formSubmitted}>
	<label for="search">Search</label>
	<input bind:value={search} id="search" name="search" />
	<button type="submit">Go</button>
</form>
{#if loading}
	<img src="https://media2.giphy.com/media/l3nWhI38IWDofyDrW/giphy.gif?cid=790b76115d055ab7424f75514dcb4d7a&rid=giphy.gif" alt="loading" >
{/if}

<div class="results">
	{#each gifs as gif}
		<img alt="jifs" src={gif}>
	{/each}
</div>


<style>
	.results {
		column-count: 3;
	}

	img {
		width: 100%;
		height: auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>