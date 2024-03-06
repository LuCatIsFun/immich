<script lang="ts" context="module">
  export interface SearchDisplayFilters {
    isNotInAlbum?: boolean;
    isArchive?: boolean;
    isFavorite?: boolean;
  }
</script>

<script lang="ts">
  export let filters: SearchDisplayFilters;
</script>

<div id="display-options-selection" class="text-sm">
  <p class="immich-form-label">展示设置</p>

  <div class="flex flex-wrap gap-x-5 gap-y-2 mt-1">
    <label class="flex items-center gap-2">
      <input type="checkbox" class="size-5 flex-shrink-0" bind:checked={filters.isNotInAlbum} />
      <span class="pt-1">不属于任何专辑</span>
    </label>

    <label class="flex items-center gap-2">
      <input type="checkbox" class="size-5 flex-shrink-0" bind:checked={filters.isArchive} />
      <span class="pt-1">已归档</span>
    </label>

    <label class="flex items-center gap-2">
      <input type="checkbox" class="size-5 flex-shrink-0" bind:checked={filters.isFavorite} />
      <span class="pt-1">收藏</span>
    </label>
  </div>
</div>
