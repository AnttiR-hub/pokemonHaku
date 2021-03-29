<script>
  export let disabled = false;
</script>

<button on:click {disabled}> <slot /> </button>

<style>
  button {
    border: 2px solid rgb(156, 155, 155);
  }
</style>
