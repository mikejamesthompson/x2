<script>
  import page from 'page';

  import Base from "./pages/base.svelte";
  import multiply from "./pages/multiply.svelte";
  import stats from "./pages/stats.svelte";

  // set default component
  let current = multiply;

  // Map routes to pages. If a route is hit the current
  // reference is set to the route's component
  page('/', () => (current = multiply));
  page('/stats', () => (current = stats));
  page.start();

</script>

<Base page={current} />