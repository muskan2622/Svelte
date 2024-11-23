<!-- SearchBar.svelte -->
<script>
  let searchTerm = '';

  function handleSearch() {
    // Handle search logic here, such as emitting an event or calling a function in the parent component
    console.log('Search term:', searchTerm);
  }
</script>

<div class="flex items-center">
  <input type="text" placeholder="Enter city name or zip code" bind:value={searchTerm} class="px-4 py-2 rounded-lg border" />
  <button on:click={handleSearch} class="px-4 py-2 ml-2 bg-blue-500 text-white rounded-lg">Search</button>
</div>
