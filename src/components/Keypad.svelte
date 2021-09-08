<script>
  export let pressedKey;
  export let pressKey;
  let keys = ['1', '2', '3', '4', '5', '6', '7', '8', '9', 'Backspace', '0', 'Enter'];
</script>

<div class="keypad">
  {#each keys as key}
  <button 
        class:selected={key==pressedKey}
        class:btn={key=='Enter'}
        class:backspace={key=='Backspace'}
        on:click={() => pressKey(key)}>
    {#if key=='Backspace'}
      Delete
    {:else}
      {key}
    {/if}
  </button>
  {/each}
</div>

<style>

  .keypad {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  .keypad button {
    border: 0;
    border-radius: 4px;
    cursor: pointer;
    flex: 0 0 32%;
    padding: 1rem 0.6rem;
    touch-action: manipulation;
  }

  .keypad button.backspace {
    background: #e3e3e3;
  }

  .keypad button.selected {
    background-color: #dbd6d7;
  }

  .keypad button.btn.selected {
    background: #CE003F
  }

</style>