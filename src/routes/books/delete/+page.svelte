<script>
    import { onMount } from 'svelte';
    import 'bootstrap/dist/css/bootstrap.min.css';
  
    let books = [];
    let deletingBook = null;
  
    onMount(async () => {
      await fetchBooks();
    });
  
    const fetchBooks = async () => {
      const response = await fetch('https://localhost:7125/api/books');
      if (response.ok) {
        books = await response.json();
      }
    };
  
    const deleteBook = (id) => {
      deletingBook = books.find(book => book.id === id);
    };
  
    const confirmDelete = async () => {
      const response = await fetch(`https://localhost:7125/api/books/${deletingBook.id}`, {
        method: 'DELETE',
      });
  
      if (response.ok) {
        await fetchBooks();
        deletingBook = null;
      } else {
        console.error('Failed to delete the book');
      }
    };
  
    const cancelDelete = () => {
      deletingBook = null;
    };
  </script>
  
  <main class="container mt-5">
    <h1 class="mb-4">Delete Books</h1>
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
              <button on:click={() => deleteBook(book.id)} class="btn btn-danger btn-sm">Delete</button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  
    {#if deletingBook}
      <div class="mb-4">
        <h2>Confirm Deletion</h2>
        <p>Are you sure you want to delete the book "{deletingBook.title}"?</p>
        <button on:click={confirmDelete} class="btn btn-danger">Yes, Delete</button>
        <button on:click={cancelDelete} class="btn btn-secondary">Cancel</button>
      </div>
    {/if}
  </main>

  