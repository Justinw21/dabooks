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
						'&key=AIzaSyBG3tXPoXEbCAvMw9YK3eEMeRXj0vtj-LM&maxResults=40'
				)
				.then((res) => {
					// This will get the responses data and cast it
					// to an APIResponse interface.
					const data: APIResponse = res.data;
					// console.log(data.items[0].volumeInfo.title);
					bookData = data.items;
				})
				.catch((err) => console.log(err));
		}
	}
</script>

<h1>Book Search</h1>
<!-- <p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->

<div class="search">
	<input
		type="text"
		placeholder="Enter Book Name"
		bind:value={search}
		on:input={inputChange}
		on:keypress={searchBook}
	/>
	<button><i class="fas fa-search" /></button>
</div>

<div class="container">
	{#each bookData as book, i}
		<div class="book-item">
			<img src={book.volumeInfo.imageLinks.smallThumbnail} alt="" />
			<h3 class="title">{book.volumeInfo.title}</h3>
			<p class="desc">{book.volumeInfo.description}</p>
		</div>
	{/each}
</div>

<style>
	.container {
		width: 65%;
		margin: 0 auto;
		display: grid;
		grid-template-columns: repeat(3, minmax(200px, 0.5fr));
		grid-gap: 20px;
		/* box-sizing: border-box; */
	}

	.book-item img {
		width: 65%;
	}
</style>
