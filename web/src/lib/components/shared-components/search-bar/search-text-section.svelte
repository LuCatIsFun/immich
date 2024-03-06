<script lang="ts">
  export let filename: string | undefined;
  export let context: string | undefined;

  enum TextSearchOptions {
    Context = 'context',
    Filename = 'filename',
  }

  let selectedOption = filename ? TextSearchOptions.Filename : TextSearchOptions.Context;

  $: {
    if (selectedOption === TextSearchOptions.Context) {
      filename = undefined;
    } else {
      context = undefined;
    }
  }
</script>

<div class="flex gap-5">
  <label class="immich-form-label" for="context">
    <input type="radio" name="context" id="context" bind:group={selectedOption} value={TextSearchOptions.Context} />
    <span>背景</span>
  </label>

  <label class="immich-form-label" for="file-name">
    <input
      type="radio"
      name="file-name"
      id="file-name"
      bind:group={selectedOption}
      value={TextSearchOptions.Filename}
    />
    <span>文件名</span>
  </label>
</div>

{#if selectedOption === TextSearchOptions.Context}
  <input
    class="immich-form-input hover:cursor-text w-full !mt-1"
    type="text"
    id="context"
    name="context"
    placeholder="海上的日出"
    bind:value={context}
  />
{:else}
  <input
    class="immich-form-input hover:cursor-text w-full !mt-1"
    type="text"
    id="file-name"
    name="file-name"
    placeholder="文件名或拓展名，如：IMG_1234.JPG 或 PNG"
    bind:value={filename}
  />
{/if}
