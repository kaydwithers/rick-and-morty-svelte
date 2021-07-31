<script>
  import { createEventDispatcher, onMount } from "svelte";
  import { API_URL } from "../js/const";

  import Episodes from "./Episodes.svelte";
  import Loading from "./Loading.svelte";

  export let characterId = null;

  const dispatch = createEventDispatcher();

  let character = null;
  let error = null;
  let isLoading = false;

  /**
   * Get the character API response.
   *
   * @param {String} characterId - The characterId of the character.
   * @return {Promise}
   */
  function getCharacter(characterId) {
    isLoading = true;
    error = null;

    fetch(API_URL + characterId)
      .then((response) => {
        if (response.ok) {
          return response.json();
        }
      })
      .then((data) => {
        isLoading = false;
        character = data;
      })
      .catch((error) => {
        console.error(`Failed getCharacter(): ${error}`);
        isLoading = false;
        error = error;
      });
  }

  /**
   * Handle the click of the close button.
   */
  function handleClose() {
    dispatch("closeModal");
  }

  onMount(() => {
    getCharacter(characterId);
  });
</script>

<div class="modal">
  <div class="modal__overlay" on:click={handleClose} />

  <div class="modal__content">
    <button class="close" on:click={handleClose}>✖</button>

    {#if isLoading}
      <Loading />
    {:else if !isLoading && error}
      <p class="error">Something went wrong. Please try again.</p>
    {:else if !isLoading && !character}
      <p class="error">No matching character found.</p>
    {:else}
      <div v-else>
        <img alt={character.name} src={character.image} />

        <div class="heading">
          <h2>{character.name}</h2>
        </div>

        <div class="paragraph">
          <p><strong>Species:</strong> {character.species}</p>
          <p><strong>Location:</strong> {character.location.name}</p>
          <p><strong>Status:</strong> {character.status}</p>
        </div>

        <Episodes {character} />
      </div>
    {/if}
  </div>
</div>

<style>
  .error {
    margin: 4rem 2rem;
    text-align: center;
  }

  .modal {
    position: fixed;
    display: flex;
    height: 100%;
    align-items: center;
    justify-content: center;
    left: 0;
    top: 0;
    width: 100%;
    z-index: 1;
  }

  .modal__overlay {
    position: absolute;
    background-color: #000;
    height: 100%;
    left: 0;
    opacity: 0.5;
    width: 100%;
  }

  .modal__content {
    background-color: var(--background-color-primary);
    border-radius: var(--border-radius);
    box-shadow: var(--box-shadow);
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-height: 50%;
    padding: 4rem 2rem;
    position: relative;
    text-align: center;
    width: 100%;
    z-index: 1;
  }

  .modal__content .close {
    cursor: pointer;
    font-size: 2rem;
    padding: 1rem;
    position: absolute;
    right: 0;
    top: 0;
  }

  .modal__content img {
    aspect-ratio: 1 / 1;
    object-fit: cover;
    margin-bottom: 1.5rem;
  }

  .modal__content .heading {
    margin-bottom: 1rem;
  }

  .modal__content .paragraph {
    margin-bottom: 2rem;
  }

  .modal__content p {
    margin-bottom: 0.5rem;
  }

  .modal__content button {
    margin-bottom: 2rem;
  }

  @media (min-width: 768px) {
    .modal__content {
      margin-left: 2rem;
      margin-right: 2rem;
    }
  }

  @media (min-width: 1024px) {
    .modal__content {
      width: 35%;
    }

    .modal__content .close {
      padding: 2rem;
    }
  }
</style>
