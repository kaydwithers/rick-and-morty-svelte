<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
</script>

<footer>
  <button on:click={() => dispatch("previousPage")}>⬅</button>
  <button on:click={() => dispatch("nextPage")}>➡</button>
</footer>

<style>
  footer {
    display: flex;
    justify-content: center;
    margin-top: auto;
    padding: 4rem 1rem;
  }

  button {
    font-size: 4rem;
    margin-left: 2rem;
    margin-right: 2rem;
  }
</style>
