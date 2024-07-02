<script lang="ts">
  export let tags: string[] = [];
  export let selectedTag: string = '';
  export let onSelectTag: (tag: string) => void;

  const selectTag = (tag: string) => {
    onSelectTag(tag);
  };
</script>

<div class="tag-container">
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
</div>

<style>
  .tag-container {
    position: sticky;
    top: 0;
    background-color: white;
    z-index: 10;
    padding: 10px;
    border-bottom: 1px solid #ddd;
    display: flex;
    justify-content: center;
  }
  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
  }
  .tags button {
    margin: 5px;
    padding: 5px 10px;
    cursor: pointer;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  .tags button.selected {
    background-color: #333;
    color: #fff;
  }
</style>
