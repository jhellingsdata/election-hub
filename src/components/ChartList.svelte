<script lang="ts">
  import ChartItem from './ChartItem.svelte';
  export let charts: { id: string; title: string; file: string }[] = [];
</script>

<div class="chart-list">
  {#each charts as chart}
    <ChartItem {chart} />
  {/each}
</div>

<style>
  .chart-list {
    display: flex;
    flex-direction: column;
  }
</style>
