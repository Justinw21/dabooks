<script lang="ts">
	import axios from 'axios';
	import type { APIResponse, Item } from '../../types';
	import '../../styles/mainpage.css';
	import { destroy_component } from 'svelte/internal';

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
		<div class = "header-container">
			<h1>Book Search</h1>
			<h2> <a href = "/profile"> Profile </a> </h2>
			<h2> <a href = "/recommendation"> Recommendations </a> </h2> 
			<h2> <a href = "/about"> About Us </a> </h2>
		</div>
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
						{#if book.volumeInfo.authors != undefined}
							<span class="author">by {book.volumeInfo.authors.join(', ')}</span>
						{:else}
							<span class="author">Unknown</span>
						{/if}
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
					{#if book.volumeInfo.imageLinks != undefined}
					<img src={book.volumeInfo.imageLinks.thumbnail} alt="" />
					{:else}
					<img src = '/Logo.png' alt="">
					{/if}
					<h3 class="title">{book.volumeInfo.title}</h3>
					{#if book.volumeInfo.authors != undefined}
						<p class="author">{book.volumeInfo.authors.join(', ')}</p>
					{:else}
						<p class = "author">Unknown</p>
					{/if}

					{#if book.volumeInfo.description != undefined}
						{#if book.volumeInfo.description.length >= 500}
							<p class="desc">{book.volumeInfo.description.slice(0, 500)}...</p>
						{:else}
							<p class="desc">{book.volumeInfo.description}</p>
						{/if}
						{:else}
							<p class="desc"> No description avaliable</p>
					{/if}
				</div>
			{/each}
		</div>
	</div>
</div>