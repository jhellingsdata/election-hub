<script lang="ts">
  /**
   * This component renders a Vega-Lite chart using the vega-embed library.
   * 
   * The `chart` prop contains the information about the chart to render, including its title and file path.
   * 
   * The `renderChart` function handles the chart rendering. It first checks if there's an existing chart view and 
   * finalises it to clean up the old chart before rendering the new one. This ensures that the chart updates correctly 
   * whenever the `chart` prop changes.
   * 
   * The `onMount` lifecycle hook runs `renderChart` when the component is first mounted.
   * The `afterUpdate` lifecycle hook runs `renderChart` after each update to ensure the chart is updated whenever the 
   * `chart` prop changes.
   * The `onDestroy` lifecycle hook ensures that the old chart is cleaned up when the component is destroyed.
   */
  import { onMount, onDestroy, afterUpdate } from 'svelte';
  export let chart: { id: string; title: string; file: string };

  let chartElement: HTMLDivElement;

  // onMount(async () => {
  //   const vegaEmbed = await import('vega-embed');
  //   vegaEmbed.default(chartElement, chart.file);
  // });

  let view: any;  // Vega view instance type

  const renderChart = async () => {
    const vegaEmbed = await import('vega-embed');
    if (view) {
      view.finalize(); // Clean up the previous view
    }
    view = await vegaEmbed.default(chartElement, chart.file);
  };

  onMount(renderChart);

  afterUpdate(() => {
    renderChart();
  });

  onDestroy(() => {
    if (view) {
      view.finalize();
    }
  });

</script>

<div class="chart-item">
  <!-- <h3>{chart.title}</h3> -->
  <div bind:this={chartElement} class="chart-container"></div>
</div>

<style>
  .chart-item {
    margin: 20px 0;
  }
  .chart-container {
    width: 100%;
    max-width: 600px;
    height: 100%;
  }
</style>
