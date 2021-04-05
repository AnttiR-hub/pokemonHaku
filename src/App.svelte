<script>
  //importataan komponentit ja transitio
  import Painike from './Painike.svelte';
  import HaetutLista from './HaetutLista.svelte';
  import Info from './Info.svelte';
  import Spinner from './Spinner.svelte';
  import { fly } from 'svelte/transition';

  //määritellään pokemoni muuttuja, jolla haku suoritetaan ja, jota voidaan välittää propsina komponenteille
  let pokemoni;

  //toiminnallisuus jolla saadaan togglettua näkyykö pokemonista haettu lisäinfo vai ei
  let showInfo = false;
  function toggleInfo() {
    showInfo = !showInfo;
  }

  //await lohkoa varten määritellään pari muuttujaa
  let hae = false;
  let haku;

  //async funktio jolla tehdään haku PokeAPIsta(https://pokeapi.co/), palauttaa json muodossa dataa jos haulla löytyy pokemon
  const getPokemon = async (pokemoni) => {
    const response = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${pokemoni}`
    );
    if (!response.ok) {
      throw new Error('Haettua pokemonia ei löytynyt');
    }
    return await response.json();
  };
</script>

<body>
  <header>
    <h1>Pokémon-haku</h1>
  </header>
  <p>Hae pokémoni nimeltä:</p>

  <!--inputin arvolla suoritetaan haku PokeAPIsta-->
  <input
    placeholder="Haku"
    type="text"
    bind:this={pokemoni}
    on:click={() => {
      showInfo = false;
    }}
  />
  <Painike
    on:click={() => {
      hae = true;
      haku = getPokemon(pokemoni.value);
    }}>Hae</Painike
  >

  {#if hae}
    <!--await-lohko fetchin käsittelyyn ja tulosten tulostamiseen-->
    {#await haku}
      <div>Loading...</div>
      <Spinner />
    {:then responseData}
      <p
        class="isoKirjain"
        id="tulos"
        in:fly={{ y: 200, duration: 500 }}
      >
        {responseData.name} on pokémon!
      </p>
      <button on:click={toggleInfo} in:fly={{ y: 200, duration: 500 }}
        >Näytä lisätietoja</button
      >
      <HaetutLista pokemoni={pokemoni.value} />
    {:catch error}
      <p id="red">Haettua pokémonia ei löytynyt :(</p>
    {/await}
  {/if}

  {#if showInfo}
    <Info pokemoni={pokemoni.value} />
  {/if}
</body>

<style>
  #red {
    color: red;
  }
  #tulos {
    font-size: 1.5em;
  }

  .isoKirjain::first-letter {
    text-transform: capitalize;
  }
  body {
    background-color: rgb(185, 255, 183);
  }
  header {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #2aff3c;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 4em;
    font-weight: 100;
    background-color: #3d701f;
    width: 30%;
    padding: 0.3em;
    border-radius: 0.5em;
    box-shadow: 7px 10px rgb(128, 11, 11);
  }

  @media (min-width: 640px) {
    header {
      max-width: none;
    }
  }
</style>
