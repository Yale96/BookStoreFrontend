<script lang="ts">
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let books: Array<Book> = [];

  // Get data from the C# backend when the component mounts
  onMount(async () => {
      const response = await fetch('https://localhost:7125/api/books');
      if (response.ok) {
          books = await response.json();
      }
  });
</script>

<main class="container mt-5">
    <h1 class="mb-4">Get the Full Book List</h1>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Title</th>
          <th scope="col">Description</th>
          <th scope="col">Author</th>
          <th scope="col">Number of Pages</th>
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
          </tr>
        {/each}
      </tbody>
    </table>
  </main>

<style>
  main {
      text-align: center;
      margin: 2em;
  }
</style>