<script lang="ts">
  import { onMount } from 'svelte';
  export let chart: { id: string; title: string; file: string };

  let chartElement: HTMLDivElement;

  onMount(async () => {
    const vegaEmbed = await import('vega-embed');
    vegaEmbed.default(chartElement, chart.file);
  });
</script>

<div class="chart-item">
  <h3>{chart.title}</h3>
  <div bind:this={chartElement}></div>
</div>

<style>
  .chart-item {
    margin: 20px 0;
  }
</style>
