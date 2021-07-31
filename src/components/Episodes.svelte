<script>
  import { fade } from "svelte/transition";

  import Button from "./Button.svelte";

  export let character = null;

  let isExpanded = false;

  /**
   * Handle the click of the accordion button.
   */
  function handleAccordion() {
    isExpanded = !isExpanded;
  }
</script>

<Button on:click={handleAccordion}>
  Episodes {#if isExpanded}⬆{:else}⬇{/if}
</Button>

{#if isExpanded}
  <div class="episodes" transition:fade={{ duration: 500 }}>
    <p>Episodes: {character.episode.length}</p>
  </div>
{/if}

<style>
  .episodes {
    margin-top: 2rem;
  }
</style>
