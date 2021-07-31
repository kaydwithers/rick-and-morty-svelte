<script>
  import { onMount } from "svelte";
  import { API_URL } from "./js/const";

  import Characters from "./components/Characters/index.svelte";
  import Footer from "./components/Footer.svelte";
  import Header from "./components/Header.svelte";
  import Loading from "./components/Loading.svelte";
  import Modal from "./components/Modal.svelte";

  let characterId = null;
  let characters = [];
  let currentPage = 1;
  let error = null;
  let isLoading = false;
  let isModalOpen = false;
  let searchText = null;

  /**
   * Get the API response.
   *
   * @param {String} param - The API url param.
   * @return {Promise}
   */
  function getCharacters(param = "") {
    isLoading = true;
    error = null;

    fetch(API_URL + param)
      .then((response) => {
        if (response.ok) {
          return response.json();
        }
      })
      .then((data) => {
        isLoading = false;
        characters = data.results;
      })
      .catch((error) => {
        console.error(`Failed getCharacters(): ${error}`);
        isLoading = false;
        error = error;
      });
  }

  /**
   * Handle the click of a character selection.
   *
   * @param {String} id - The ID of the character.
   */
  function handleCharacterSelect(event) {
    characterId = event.detail.id;
    isModalOpen = true;
  }

  /**
   * Handle the click of the next page button.
   */
  function handleNextPage() {
    currentPage++;
    getCharacters(`?page=${currentPage}`);
  }

  /**
   * Handle the click of the previous page button.
   */
  function handlePreviousPage() {
    if (currentPage >= 1) {
      currentPage--;
      getCharacters(`?page=${currentPage}`);
    }
  }

  /**
   * Handle the search input.
   */
  function handleSearch() {
    getCharacters(`?name=${searchText}`);
  }

  onMount(() => {
    getCharacters();
  });
</script>

{#if isModalOpen}
  <Modal {characterId} on:closeModal={() => (isModalOpen = false)} />
{/if}

<Header bind:value={searchText} on:inputValue={handleSearch} />

<main>
  {#if isLoading}
    <Loading />
  {:else if !isLoading && error}
    <p class="error">Something went wrong. Please try again.</p>
  {:else if !isLoading && (!characters || characters.length === 0)}
    <p class="error">No characters found.</p>
  {:else}
    <Characters {characters} on:characterSelect={handleCharacterSelect} />
  {/if}
</main>

<Footer on:nextPage={handleNextPage} on:previousPage={handlePreviousPage} />

<style>
  .error {
    margin: 4rem 2rem;
    text-align: center;
  }
</style>
