<script>
  import Search from "./Search.svelte";
  import SearchResults from "./SearchResults.svelte";
  import LoadingIndicator from "./LoadingIndicator.svelte";

  const UNSPLASH_ACCESS_KEY = "EJ49yYYs-cEuoMtG4Q9dtmWEOyp3A4nJAzUaQV1qqTA";

  let searchQuery = "";
  let searchTerm = null;
  let totalPages = null;
  let searchResults = [];
  let nextPage = 1;
  let isLoading = false;

  function handleSubmit() {
    searchTerm = searchQuery.trim();
    searchResults = [];
    totalPages = null;
    nextPage = 1;

    if (!searchTerm) return;

    searchUnsplash();
  }

  function searchUnsplash() {
    isLoading = true;

    const endpoint = `https://api.unsplash.com/search/photos?query=${searchTerm}&page=${nextPage}&per_page=28&client_id=${UNSPLASH_ACCESS_KEY}`;

    fetch(endpoint)
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok.");
        }
        return response.json();
      })
      .then((data) => {
        if (data.total === 0) {
          alert("No photoes were found for your search query.");
          return;
        }

        searchResults = [...searchResults, ...data.results];
        totalPages = data.total_pages;

        if (nextPage < totalPages) {
          nextPage += 1;
        }
      })
      .catch(() => alert("Something went wrong."))
      .finally(() => {
        isLoading = false;
      });
  }
</script>

<main class="App">
  <h1>Unsplash Search App</h1>

  <Search bind:query={searchQuery} {handleSubmit} />

  <SearchResults results={searchResults} />

  <div class="loading-indicator">
    {#if isLoading}
      <LoadingIndicator />
    {/if}
  </div>
</main>

<style>
  .App {
    width: 100%;
    max-width: 1500px;
    padding: 20px;
    margin: 0 auto 50px;
    text-align: center;
  }

  h1 {
    font-size: 50px;
    margin-top: 30px;
    margin-bottom: 30px;
  }
</style>
