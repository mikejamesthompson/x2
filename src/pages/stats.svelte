<script>
  import { fade } from 'svelte/transition';
	import { onMount } from 'svelte';
  let stats = [];

  onMount(() => {
    stats = JSON.parse(localStorage.getItem('x2_stats') || '[]');
    console.log(stats);
  });
</script>

<div class="container" in:fade>
  <table class="stats">
    <thead>
      <tr>
        <th>Date</th>
        <th>Times tables</th>
        <th>Right</th>
        <th>Wrong</th>
      </tr>
    </thead>
  {#each stats as stat}
    <tr>
      <td>{stat.date}</td>
      <td><strong>{stat.numbers.join(", ")}</strong></td>
      <td>{stat.right}</td>
      <td>{stat.wrong}</td>
    </tr>
  {/each}
  </table>
</div>

<style>
  .stats {
    border-collapse: collapse;
    width: 100%;
  }

  .stats th, .stats td {
    border: 1px solid grey;
    padding: 0.5em;
  }
</style>