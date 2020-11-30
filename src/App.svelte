<script>
	let search;
	let loading = false;
	$: bgImage;
	const API_URL = `https://api.giphy.com/v1/gifs/search?api_key=${API_KEY}&rating=pg&q=`;

	let gifs = [];

	async function formSubmitted() {
		loading = true;
		gifs = []; //reset array on new search
		const url = `${API_URL}${search}`;
		const response = await fetch(url);
		const json = await response.json();
		gifs = json.data.map(gif => gif.images.fixed_height.url);
		loading = false;
		bgImage = `style=background-image: url('${gifs[0]}')}`
	}





	let index = Math.floor(Math.random() * gifs.length);

</script>

<div class="site-wrapper" {{bgImage}}>
	<div class="site-wrapper-inner">
		<div class="container">
			<div class="masthead clearfix">
				<div class="container inner">
					<h3 class="masthead-brand">JIFS</h3>
				</div>
			</div>
			<div class="inner cover">
				<form class="form-group" action="" on:submit|preventDefault={formSubmitted}>
					<label class="form-check-label" for="search">{search}</label>
					<input class="form-control" bind:value={search} id="search" name="search" />
					<div class="buttonContainer">
						<button class="btn btn-primary" type="submit">Search</button>
						<button class="btn btn-primary" type="submit">Shuffle</button>
					</div>
				</form>
			</div>
		</div>
	</div>

</div>

{#if loading}
	<img src="https://media2.giphy.com/media/l3nWhI38IWDofyDrW/giphy.gif?cid=790b76115d055ab7424f75514dcb4d7a&rid=giphy.gif" alt="loading" >
{/if}

<div class="results">
	{#each gifs as gif}
		<img alt="jifs" src={gifs[0]}>
	{/each}
</div>


<style lang="scss">
	form {
		text-align: center;
		margin: 5%;
		label {
			display: block;
		}
		input {
			width: 33%;
			margin: auto;
		}
		.buttonContainer {
			display: inline-flex;
			button {
				display: block;
				width: 5em;
				margin: 1em 1em auto;
			}
		}
	}
	.results {
		column-count: 3;
		img {
			width: 100%;
			height: auto;
		}
	}


	/*
 * Globals
 */
	/* Links */
	a,
	a:focus,
	a:hover {
		color: #fff;
	}

	/* Custom default button */
	.btn-default,
	.btn-default:hover,
	.btn-default:focus {
		color: #333;
		text-shadow: none; /* Prevent inheritence from `body` */
		background-color: #fff;
		border: 1px solid #fff;
	}
	/*
     * Base structure
     */
	html,
	body {
		height: 100%;
		background-color: #fff;
	}
	body {
		color: #fff;
		text-align: center;
		text-shadow: 0 1px 3px rgba(0,0,0,.5);
	}
	/* Extra markup and styles for table-esque vertical and horizontal centering */
	.site-wrapper {
		display: table;
		width: 100%;
		height: 600px;
		//background-color: #333;
		/*height: 100%; */
		/*min-height: 100%;*/
	}
	.site-wrapper-inner {
		display: table-cell;
		vertical-align: top;
	}
	.cover-container {
		margin-right: auto;
		margin-left: auto;
	}

	/* Padding for spacing */
	.inner {
		padding: 30px;
		text-align: -webkit-center;
		text-align: center;
	}

	@media (min-width: 768px) {
		.masthead-brand {
			float: left;
		}
		.masthead-nav {
			float: right;
		}
	}


	/*
     * Cover
     */

	.cover {
		padding: 0 20px;
	}
	.cover .btn-lg {
		padding: 10px 20px;
		font-weight: bold;
	}

	/*
     * Affix and center
     */

	@media (min-width: 768px) {
		/* Pull out the header and footer */
		.masthead {
			position: fixed;
			top: 0;
		}
		/* Start the vertical centering */
		.site-wrapper-inner {
			vertical-align: middle;
		}

	}



</style>