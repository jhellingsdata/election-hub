<script lang="ts">
  export let tags: string[] = [];
  export let selectedTag: string = '';
  export let onSelectTag: (tag: string) => void;

  const selectTag = (tag: string) => {
    onSelectTag(tag);
  };
</script>

<div class="tags">
  {#each tags as tag}
    <button
      class:selected={tag === selectedTag}
      on:click={() => selectTag(tag)}
    >
      {tag}
    </button>
  {/each}
</div>

<style>
  .tags button {
    margin: 5px;
    padding: 5px 10px;
    cursor: pointer;
  }
  .tags button.selected {
    background-color: #333;
    color: #fff;
  }
</style>
