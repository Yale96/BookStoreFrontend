<script>
    import { onMount } from "svelte";
    import 'bootstrap/dist/css/bootstrap.min.css';
  
    let books = [];
  
    // Fetch data from the C# backend when the component mounts
    onMount(async () => {
      const response = await fetch("https://localhost:7125/api/books");
      if (response.ok) {
        books = await response.json();
      }
    });
  </script>
  
  <main>
    <h1>Book List</h1>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Title</th>
          <th scope="col">Description</th>
          <th scope="col">Author</th>
          <th scope="col">Number of Pages</th>
          <!-- Add more columns for other properties -->
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
            <!-- Add more cells for other properties -->
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
  