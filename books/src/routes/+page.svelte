<script lang="ts">
	import axios from 'axios';
	import type { APIResponse, Item } from './types';

	let search = '';
	let bookData: Item[] = [];

	function inputChange(event: Event) {
		search = (event.target as HTMLInputElement).value;
	}

	function searchBook(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			axios
				.get(
					'https://www.googleapis.com/books/v1/volumes?q=' +
						search +
						'&key=AIzaSyBG3tXPoXEbCAvMw9YK3eEMeRXj0vtj-LM&maxResults=10'
				)
				.then((res) => {
					// This will get the responses data and cast it
					// to an APIResponse interface.
					const data: APIResponse = res.data;
					bookData = data.items;
				})
				.catch((err) => console.log(err));
		}
	}

	function searchBookButton(event: MouseEvent) {
		axios
			.get(
				'https://www.googleapis.com/books/v1/volumes?q=' +
					search +
					'&key=AIzaSyBG3tXPoXEbCAvMw9YK3eEMeRXj0vtj-LM&maxResults=10'
			)
			.then((res) => {
				// This will get the responses data and cast it to an APIResponse interface.
				const data: APIResponse = res.data;
				bookData = data.items;
			})
			.catch((err) => console.log(err));
	}
</script>

<div id="app">
	<div class="left">
		<h1>Book Search</h1>
	</div>

	<div class="right">
		<div class="search">
			<input
				type="text"
				placeholder="Enter Book Name"
				bind:value={search}
				on:input={inputChange}
				on:keypress={searchBook}
			/>
			<button on:click={searchBookButton}
				><i class="fa-solid fa-magnifying-glass" style="color: #ffffff;" /></button
			>
		</div>

		<div class="book-container">
			{#each bookData as book, i}
				<div class="book-item" id="tooltip">
					<div id="tooltiptext">
						<span class="title">{book.volumeInfo.title}</span>
						<span class="author">by {book.volumeInfo.authors.join(', ')}</span>
						<p>
							Page Count: {book.volumeInfo.pageCount}
							<br />
							Publisher:
							{#if book.volumeInfo.publisher === undefined}
								Not Available
							{:else}
								{book.volumeInfo.publisher}
							{/if}
							<br />
							Genre: {book.volumeInfo.categories}
							<br />
							Language: {book.volumeInfo.language}
							<br />

							Price:
							{#if book.saleInfo.retailPrice?.amount === undefined}
								Not Available
							{:else}
								{book.saleInfo.retailPrice?.amount} USD
							{/if}
						</p>
					</div>

					<img src={book.volumeInfo.imageLinks.thumbnail} alt="" />
					<h3 class="title">{book.volumeInfo.title}</h3>
					<p class="author">{book.volumeInfo.authors.join(', ')}</p>
					{#if book.volumeInfo.description.length >= 500}
						<p class="desc">{book.volumeInfo.description.slice(0, 500)}...</p>
					{:else}
						<p class="desc">{book.volumeInfo.description}</p>
					{/if}
				</div>
			{/each}
		</div>
	</div>
</div>

<style>
	#app {
		display: flex;
		min-height: 100vh;
		min-width: 100vw;
	}
	h1 {
		font-family: 'Indie Flower', cursive;
		font-size: xxx-large;
		color: rgb(117, 72, 20);
		background-color: rgb(219, 204, 186);
		border-radius: 20px;
		margin: 10%;
		border-style: dashed;
		border-width: 5px;
	}
	.right {
		float: right;
		width: 85%;
	}

	.left {
		width: 15%;
		float: left;
		text-align: center;
		background-image: url('./src/img/books.jpg');
	}

	.search {
		padding: 25px;
		text-align: center;
	}
	input {
		border-radius: 15px;
		border-color: rgb(133, 105, 29);
		padding: 1%;
		font-family: 'Montserrat', sans-serif;
		width: 80%;
		text-align: center;
	}

	button {
		background-color: transparent;
		border: none;
		cursor: pointer;
	}

	.book-container {
		display: grid;
		grid-template-columns: repeat(3, minmax(200px, 0.5fr));
		grid-gap: 30px;
		padding: 5%;
	}

	.book-item {
		background-color: rgb(252, 247, 241);
		border-radius: 20px;
		padding: 5%;
		text-align: center;
		font-family: 'Montserrat', sans-serif;
	}

	#tooltip {
		position: relative;
		display: block;
	}

	#tooltip #tooltiptext {
		visibility: hidden;
		width: 75%;
		height: auto;
		background-color: rgb(250, 250, 250);

		padding: 2%;
		color: #000000;
		text-align: center;
		text-align: left;
		font-size: small;
		font-family: 'Montserrat', sans-serif;

		border-radius: 20px;
		border-style: solid;
		border-width: 5px;
		border-color: rgb(117, 72, 20);

		position: absolute;
		z-index: 1;
		top: 5px;
		left: 102%;
	}

	#tooltip:hover #tooltiptext {
		visibility: visible;
	}

	.title {
		margin-bottom: 0%;
		font-weight: 1000;
	}

	.desc {
		text-align: left;
		font-size: small;
	}
	.author {
		font-size: small;
		font-style: italic;
	}
</style>
