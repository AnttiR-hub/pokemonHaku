<script>
  import haetut from './hautStore';
  import { onMount } from 'svelte';
  export let pokemoni;
  onMount(() => {
    haetut.update((haetutPoket) => [...haetutPoket, pokemoni]);
  });
</script>

<div class="haetut">
  <p>Aikaisemmat haut:</p>
  {#each $haetut as haettu}
    <p class="isoKirjain">
      {haettu}
    </p>
  {/each}
</div>

<style>
  .isoKirjain::first-letter {
    text-transform: capitalize;
  }

  .haetut {
    background-color: blanchedalmond;
    border: 1px black solid;
    width: 20%;
  }
</style>
