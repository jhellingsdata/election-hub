<script lang="ts">
  import { onMount } from 'svelte';
  import TagList from './components/TagList.svelte';
  import ChartList from './components/ChartList.svelte';

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
      tags = [...new Set(data.flatMap(chart => chart.tags))];
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
    selectedTag = tag;
    console.log('Selected tag:', selectedTag);  // Debugging
  };

  onMount(fetchCharts);
</script>

<main>
  <TagList {tags} {selectedTag} onSelectTag={handleSelectTag} />
  <ChartList charts={filteredCharts} />
</main>

<style>
  main {
    padding: 40px 20px 20px 20px; /* Add top padding to avoid overlap with sticky tags */
  }
</style>
