<script lang="ts">
  import { flip } from 'svelte/animate';
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';
  import TagList from './components/TagList.svelte';
  import Grid from './components/Grid.svelte';
  import ChartItem from './components/ChartItem.svelte';

  type Chart = {
    id: string;
    title: string;
    tags: string[];
    file: string;
  };

  let charts: Chart[] = [];
  let tags: string[] = [];
  let selectedTag: string = '';
  let filteredCharts: Chart[] = [];

  const fetchCharts = async () => {
    try {
      const res = await fetch('/charts.json');
      const data: Chart[] = await res.json();
      console.log('Fetched charts:', data);  // Debugging
      charts = data;
      // tags = [...new Set(data.flatMap(chart => chart.tags))];
      const chartTags = [...new Set(data.flatMap(chart => chart.tags))];
      tags = [...tags, ...chartTags].filter((v, i, a) => a.indexOf(v) === i); // Ensure tags are unique
      console.log('Extracted tags:', tags);  // Debugging
    } catch (error) {
      console.error('Error fetching charts:', error);  // Debugging
    }
  };

  // const filterCharts = () => {
  //   const filteredCharts = selectedTag
  //     ? charts.filter(chart => chart.tags.includes(selectedTag))
  //     : charts;
  //   console.log('Filtered charts:', filteredCharts);  // Debugging
  //   return filteredCharts;
  // };
  $: filteredCharts = selectedTag
    ? charts.filter(chart => chart.tags.includes(selectedTag))
    : charts;


  const handleSelectTag = (tag: string) => {
    if (selectedTag === tag) {
      selectedTag = ''; // Deselect the tag if it's already selected
    } else {
      selectedTag = tag;
    }
    console.log('Selected tag:', selectedTag);
  };

  onMount(fetchCharts);
</script>

<main>
  <TagList {tags} {selectedTag} onSelectTag={handleSelectTag} />
  <Grid>
    {#each filteredCharts as chart (chart.id)}
      <div in:fade animate:flip={{ duration: 200 }} class="chart-wrapper">
        <ChartItem {chart} />
      </div>
    {/each}
  </Grid>
</main>

<style>
  main {
    padding: 40px 20px 20px 20px; /* Add top padding to avoid overlap with sticky tags */
  }
  .chart-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
