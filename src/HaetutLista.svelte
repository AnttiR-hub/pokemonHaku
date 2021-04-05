<script>
  import haetut from './hautStore';
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  export let pokemoni;

  //elinkaaren mount vaiheessa päivitetään storessa oleva lista
  onMount(() => {
    haetut.update((haetutPoket) => [...haetutPoket, pokemoni]);
  });
</script>

<div class="haetut" in:fly={{ y: 200, duration: 500 }}>
  <p>Aikaisemmat haut:</p>
  <!--tulostetaan jokainen hautStoren listasta löytyvä arvo p-elementteinä-->
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
