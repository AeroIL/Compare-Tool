<script>
  import CompareTool from "./CompareTool.svelte";
  import ComparisonTable from "./ComparisonTable.svelte";
  import { result } from "./result.js";
  let res;

  result.subscribe((value) => {
    res = value;
  });
</script>
<main>
  {#if (res === null)}
  <p>Paste the object status JSON you want to compare</p>
    <CompareTool />
  {:else}
    <ComparisonTable />
  {/if}
</main>

<style>
:global(body) {
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

  main {
  max-width: 100%;
  margin: 0 auto;
  padding: 20px;
  font-family: sans-serif;
  background-color: #0850b5;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
}

p {
  text-align: center;
  color: #fff;
}
</style>
