<script>
    import { onMount } from 'svelte';
    import 'bootstrap/dist/css/bootstrap.min.css';
  
    let books = [];
    let newBook = {
      title: '',
      description: '',
      author: '',
      numberOfPages: 0,
      // Add more properties as needed
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
  
  <main>
    <h1>Book List</h1>
    <table class="table">
      <!-- Table header as before -->
  
      <tbody>
        {#each books as book (book.id)}
          <tr>
            <!-- Table cells as before -->
          </tr>
        {/each}
      </tbody>
    </table>
  
    <h2>Add New Book</h2>
    <form on:submit|preventDefault={addBook}>
      <label>
        Title:
        <input bind:value={newBook.title} required />
      </label>
      <br />
  
      <label>
        Description:
        <input bind:value={newBook.description} required />
      </label>
      <br />
  
      <label>
        Author:
        <input bind:value={newBook.author} required />
      </label>
      <br />
  
      <label>
        Number of Pages:
        <input bind:value={newBook.numberOfPages} type="number" required />
      </label>
      <br />
  
      <!-- Add more input fields for other properties -->
  
      <button type="submit">Add Book</button>
    </form>
  </main>
  
  <style>
    main {
      text-align: center;
      margin: 2em;
    }
  </style>
  