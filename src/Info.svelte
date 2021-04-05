<script>
  import { onMount } from 'svelte';
  import { fly } from 'svelte/transition';
  // määritellään tiedot niminen objekti myöhempää varten
  let tiedot = {};
  let errorMessage;
  export let pokemoni;
  // haetaan pokemonista dataa PokeAPIsta lisätietojen tulostamista varten
  const getPokemon = async () => {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${pokemoni}`
    );
    if (!response.ok) {
      throw new Error('Haettua pokemonia ei löytynyt');
    }
    return await response.json();
  };
  onMount(() =>
    getPokemon()
      .then((data) => {
        // tarkastetaan onko haetulla pokemonilla toista tyyppiä ja
        // sijoitetaan saadusta datasta halutut lisätiedot tiedot objektiin
        if (data.types[1] !== undefined) {
          tiedot = {
            name: data.name,
            height: data.height,
            weight: data.weight,
            type1: data.types[0].type.name,
            type2: data.types[1].type.name,
          };
        } else {
          // vaihtoehtoinen tiedot objekti siltä varalta, että pokemonilla on vain yksi tyyppi
          tiedot = {
            name: data.name,
            height: data.height,
            weight: data.weight,
            type1: data.types[0].type.name,
          };
        }
      })
      .catch((err) => {
        errorMessage = err.message;
      })
  );
</script>

<div class="laatikko" transition:fly>
  <!-- tulostetaab halutut lisätiedot pokemista ja pyöristellään arvot helposti ymmärrettäviin muotoihin -->
  <h2 class="isoKirjain">{tiedot.name}</h2>
  <p>Korkeus: {Math.round(tiedot.height * 100) / 10}cm</p>
  <p>Paino: {tiedot.weight * 0.1}kg</p>
  <p>Tyypit:</p>
  <ul>
    <li class="isoKirjain">{tiedot.type1}</li>
    <!-- pokemonin toinen tyyppi tulostetaan if-lohkolla vain jos sellai on löytyy -->
    {#if tiedot.type2 !== undefined}
      <li class="isoKirjain">{tiedot.type2}</li>
    {/if}
  </ul>
</div>

<style>
  .isoKirjain::first-letter {
    text-transform: capitalize;
  }

  .laatikko {
    font-size: 2em;
    padding-left: 0.5em;
    border: 1px black solid;
    width: 50%;
    position: absolute;
    top: 10%;
    right: 5%;
  }
</style>
