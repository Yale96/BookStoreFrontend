<script>
    import { onMount } from 'svelte';
    import 'bootstrap/dist/css/bootstrap.min.css';
  
    let books = [];
    let editingBook = null;
  
    onMount(async () => {
      await fetchBooks();
    });
  
    const fetchBooks = async () => {
      const response = await fetch('https://localhost:7125/api/books');
      if (response.ok) {
        books = await response.json();
      }
    };
  
    const editBook = (id) => {
      editingBook = books.find(book => book.id === id);
    };
  
    const updateBook = async () => {
      const response = await fetch(`https://localhost:7125/api/books/${editingBook.id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(editingBook),
      });
  
      if (response.ok) {
        // Clear the form and fetch the updated list of books
        editingBook = null;
        await fetchBooks();
      } else {
        console.error('Failed to update the book');
      }
    };
  </script>
  
  <main class="container mt-5">
    <h1 class="mb-4">Edit Books</h1>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Title</th>
          <th scope="col">Description</th>
          <th scope="col">Author</th>
          <th scope="col">Number of Pages</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        {#each books as book (book.id)}
          <tr>
            <th scope="row">{book.id}</th>
            <td>{book.title}</td>
            <td>{book.description}</td>
            <td>{book.author}</td>
            <td>{book.numberOfPages}</td>
            <td>
              <button on:click={() => editBook(book.id)} class="btn btn-warning btn-sm">Edit</button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  
    {#if editingBook}
      <h2 class="mb-4">Update Book</h2>
      <form on:submit|preventDefault={updateBook}>
        <div class="mb-3">
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label class="form-label">Title:</label>
          <input type="text" class="form-control" bind:value={editingBook.title} required />
        </div>
  
        <div class="mb-3">
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label class="form-label">Description:</label>
          <input type="text" class="form-control" bind:value={editingBook.description} required />
        </div>
  
        <div class="mb-3">
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label class="form-label">Author:</label>
          <input type="text" class="form-control" bind:value={editingBook.author} required />
        </div>
  
        <div class="mb-3">
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label class="form-label">Number of Pages:</label>
          <input type="number" class="form-control" bind:value={editingBook.numberOfPages} required />
        </div>
        <button type="submit" class="btn btn-primary">Update Book</button>
      </form>
    {/if}
  </main>