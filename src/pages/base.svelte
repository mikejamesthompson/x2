<script>
  export let page;
</script>

{#if process.env.NODE_ENV === 'production'}

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ACCOUNT"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_ACCOUNT', { 'anonymize_ip': true });
</script>

{/if}

<svelte:component this={page} />