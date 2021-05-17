<script>
	import { fade, fly } from 'svelte/transition';
	import shuffle from './shuffle';
  import Keypad from './Keypad.svelte';
	
	export let numbers; // Which times tables we're doing
	export let reset;
	
	let multipliers = Array.from(Array(12).keys());

	let pairs = [];
	numbers.forEach((n) => {
		pairs = pairs.concat(
			multipliers.map((m) => [m + 1, n])
		);
	});
	pairs = shuffle(pairs);
	
	let right = 0;
	let wrong = 0;
	let i = 0;
	
	let answer = '';
	let lastAnswerWrong = null;
  let pressedKey = null;
  let keypadTimeoutID = null;

  function pressKey(num) {
    if (!isNaN(parseInt(num))) {
      answer = answer + num;
    // Handle backspace
    } else if (num === 'Backspace') {
      if (answer.length > 0) answer = answer.slice(0, -1);
    // Handle enter
    } else if (num === 'Enter') {
      check();
    } else {
      return;
    }

    pressedKey = num;
    if (typeof keypadTimeoutID === 'number') clearTimeout(keypadTimeoutID);
    keypadTimeoutID = setTimeout(() => pressedKey = null, 200);
  }
	
	function check() {
		const correct = parseInt(answer) == (pairs[i][0] * pairs[i][1])
		if (correct) {
			right += 1;
			i += 1;
			lastAnswerWrong = false;
		} else {
			wrong += 1;
			lastAnswerWrong = true;
			setTimeout(() => lastAnswerWrong = null, 500);
		}
		answer = '';
	}
</script>

<div class="container" in:fade>
	{#if i < pairs.length}
		<div class="stats">
			<p>
        {#if (pairs.length - i) == 1}
          <strong>Last one!</strong>
        {:else}
          <strong>{pairs.length - i}</strong> left
        {/if}
      </p>
			<p>
				<span class="counter">😺 {right}</span>
				<span class="counter">😿 {wrong}</span>
			</p>
		</div>
		
		<p class="question">
			{#key i}
				<strong in:fly="{{ y: -50, duration: 500 }}" out:fade|local>{pairs[i][0]} × {pairs[i][1]}</strong>
			{/key}
		</p>
    
    <p class="answer" class:shake={lastAnswerWrong} tabindex="0" autofocus on:keydown={(e) => pressKey(e.key)}>
      {#if answer}{answer}{:else}<em>Type your answer</em>{/if}
    </p>
		
		<Keypad {pressedKey} {pressKey} />

		<p><a href="/" on:click|preventDefault={reset}>Leave</a></p>
	{:else}
		<h2>All done!</h2>
		<div class="results">
			<p>😺 {right}&nbsp;&nbsp;&nbsp;😿 {wrong}</p>
		</div>

		<form on:submit|preventDefault={reset}>
			<button type="submit" class="btn">Practise again!</button>
		</form>
	{/if}
</div>

<style>
	
	.btn, .answer {
		border-radius: 6px;
		display:block;
		padding: 0.75em;
		width: 100%;
	}
	
	.btn {
		background: #FF004E;
		border: 0;
		color: #fff;
		cursor: pointer;
		font-weight: bold;
	}
	
	.answer {
    border: 2px solid #eee;
    box-sizing: border-box;
    font-size: 1.3rem;
	}

  .answer:focus {
    border-color: #FF004E;
  }
	
	.container {
		margin: 0 auto;
		max-width: 400px;
		position: relative;
		text-align: center
	}
	
	.stats {
		display: flex;
    font-size: 1.2rem;
		justify-content: space-between;
		width: 100%;
	}
	
	.stats p {
		margin: 0
	}
	
	.counter:last-of-type {
		margin-left: 1rem;
	}
	
	.question {
		border-radius: 6px;
		font-size: 2rem;
		margin: 2rem 0 1rem;
		min-height: 3rem;
		padding: 1rem 0;
		position: relative;
	}
	
	.question strong {
		position: absolute;
		left: 50%;
		top: 50%;
    transform: translate(-50%, -50%);
		width: 100%;
	}
	
	.results {
		background: #f6f6f6;
		border-radius: 6px;
		margin: 2rem 0 1rem;
		min-height: 2rem;
		padding: 2rem 0;
	}
	
	.results p {
		font-size: 2rem;
	}
	
	.shake {
		 animation: shake 0.3s; 
		 animation-iteration-count: 2;
     border-color: #FF004E;
     box-shadow: 0px 2px 12px rgba(255,0,78,0.4);
	 }

	 @keyframes shake {
		 0% { transform: translate(1px, 1px) rotate(0deg); }
		 20% { transform: translate(-3px, 0px) rotate(1deg); }
		 40% { transform: translate(1px, -1px) rotate(1deg); }
		 60% { transform: translate(-3px, 1px) rotate(0deg); }
		 80% { transform: translate(-1px, -1px) rotate(1deg); }
		 100% { transform: translate(1px, -2px) rotate(-1deg); }
	 }
	
</style>