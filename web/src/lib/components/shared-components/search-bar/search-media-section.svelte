<script lang="ts">
  import { MediaType } from './search-filter-box.svelte';

  export let filteredMedia: MediaType;
</script>

<div id="media-type-selection">
  <p class="immich-form-label">媒体类型</p>

  <div class="flex gap-5 mt-1 text-base">
    <label for="type-all" class="flex items-center gap-1">
      <input
        bind:group={filteredMedia}
        value={MediaType.All}
        type="radio"
        name="radio-type"
        id="type-all"
        class="size-4"
      />
      <span class="pt-0.5">全部</span>
    </label>

    <label for="type-image" class="flex items-center gap-1">
      <input
        bind:group={filteredMedia}
        value={MediaType.Image}
        type="radio"
        name="media-type"
        id="type-image"
        class="size-4"
      />
      <span class="pt-0.5">图片</span>
    </label>

    <label for="type-video" class="flex items-center gap-1">
      <input
        bind:group={filteredMedia}
        value={MediaType.Video}
        type="radio"
        name="radio-type"
        id="type-video"
        class="size-4"
      />
      <span class="pt-0.5">视频</span>
    </label>
  </div>
</div>
