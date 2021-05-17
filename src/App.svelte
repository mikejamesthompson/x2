<script>
	import { fade } from 'svelte/transition';
	import Multiplication from './Multiplication.svelte';
	
	let numbers = []
	let numberChoices = [2,3,4,5,6,7,8,9,10,11,12,14]
	let play = false;
	
	const start = () => play = true;
	
	function reset() {
		numbers = [];
		play = false;
	}
</script>


{#if !play}
	<div class="container" in:fade>
		<h2>Practise your times tables</h2>
    <p>Which times tables do you want practise?</p>

		<form on:submit|preventDefault={start}>
			<ol class="options">
				{#each numberChoices as num}
        <li class="option">
          <input type=checkbox bind:group={numbers} value={num} id="option-{num}" />
          <label for="option-{num}">
            {num}
          </label>
        </li>
				{/each}
			</ol>
			<button type="submit" class="btn" disabled={numbers.length==0}>Play</button>
		</form>
	</div>
{:else}

	<Multiplication bind:numbers {reset} />

{/if}

<style>
	
	.container {
		margin: 0 auto;
		max-width: 400px;
		text-align: center
	}
	
	.options {
		display: flex;
		flex-wrap: wrap;
    margin: 0;
    padding: 0;
	}
	
	.option {
		background: #f6f6f6;
		border: 1px solid #ddd;
		border-radius: 4px;
		display: block;
		flex: 1 1 25%;
		margin: 0 0.25em 0.5em;
    position: relative;
		text-align: center;
	}

  .option label {
		cursor: pointer;
    display: block;
		padding: 0.5em;
    touch-action: manipulation;
  }

  .option input {
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    border: 0;
    position: absolute;
  }

  .option input:checked ~ label {
    background-color: #dbd6d7;
    font-weight: bold;
  }
	
	.btn {
		background: #FF004E;
		border: 0;
		border-radius: 6px;
		color: #fff;
		cursor: pointer;
		display:block;
		font-weight: bold;
		padding: 0.75em;
		width: 100%;
	}
	
	.btn[disabled] {
		opacity: 0.2
	}
	
</style>