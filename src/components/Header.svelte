<script>
  import { createEventDispatcher } from "svelte";

  export let value = null;

  const dispatch = createEventDispatcher();

  let hasTheme = false;

  /**
   * Handle the click of the theme buttons.
   *
   * @param {String} theme - The theme to add.
   */
  function handleTheme(theme = null) {
    hasTheme = !hasTheme;
    theme ? (document.body.className = theme) : (document.body.className = "");
  }
</script>

<header>
  <label for="search">🔍</label>
  <input
    type="text"
    placeholder="Search"
    id="search"
    bind:value
    on:input={() => {
      dispatch("inputValue", value);
    }}
  />

  {#if hasTheme}
    <button on:click={handleTheme}>🌞</button>
  {:else}
    <button
      on:click={() => {
        handleTheme("dark");
      }}>🌚</button
    >
  {/if}
</header>

<style>
  header {
    align-items: center;
    display: flex;
    justify-content: center;
    padding: 4rem 1rem;
  }

  label {
    font-size: 2rem;
    margin-right: 1rem;
  }

  input {
    border-color: inherit;
    border-radius: 0.5rem;
    border-style: solid;
    border-width: 2px;
    font-size: 1rem;
    padding: 1rem;
    width: auto;
  }

  button {
    font-size: 2rem;
    margin-left: 1rem;
  }

  @media (min-width: 768px) {
    input {
      width: 16rem;
    }
  }
</style>
