<script lang="ts">
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let books: Array<Book> = [];
  let newBook: Book = {
      title: '',
      description: '',
      author: '',
      numberOfPages: 0,
  };

  onMount(async () => {
      await fetchBooks();
  });

  const fetchBooks = async () => {
      const response = await fetch('https://localhost:7125/api/books');
      if (response.ok) {
          books = await response.json();
      }
  };

  const addBook = async () => {
      const response = await fetch('https://localhost:7125/api/books', {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json',
          },
          body: JSON.stringify(newBook),
      });

      if (response.ok) {
          // Clear the form and fetch the updated list of books
          newBook = {
              title: '',
              description: '',
              author: '',
              numberOfPages: 0,
          };
          await fetchBooks();
      } else {
          console.error('Failed to add the book');
      }
  };
</script>

<main class="container mt-5">
  <h2 class="mb-4">Add New Book</h2>
  <form on:submit|preventDefault={addBook}>
    <div class="mb-3">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label class="form-label">Title:</label>
      <input type="text" class="form-control" bind:value={newBook.title} required />
    </div>

    <div class="mb-3">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label class="form-label">Description:</label>
      <input type="text" class="form-control" bind:value={newBook.description} required />
    </div>

    <div class="mb-3">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label class="form-label">Author:</label>
      <input type="text" class="form-control" bind:value={newBook.author} required />
    </div>

    <div class="mb-3">
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label class="form-label">Number of Pages:</label>
      <input type="number" class="form-control" bind:value={newBook.numberOfPages} required />
    </div>

    <!-- Add more input fields for other properties -->

    <button type="submit" class="btn btn-primary">Add Book</button>
  </form>
</main>


<style>
  main {
      text-align: center;
      margin: 2em;
  }
</style>
