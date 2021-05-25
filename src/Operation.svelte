<script>
  export let pair;
  export let includeAnswer;

  const answer = pair.operation === "×" 
    ? pair.multiplier * pair.base
    : pair.multiplier
</script>

{#if pair.operation === "×"}
  {pair.multiplier} × {pair.base}
{:else}
  {pair.multiplier * pair.base} ÷ {pair.base}
{/if}

{#if includeAnswer}
  = {answer}
{/if}