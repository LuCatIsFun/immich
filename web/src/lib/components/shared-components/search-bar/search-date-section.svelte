<script lang="ts" context="module">
  export interface SearchDateFilter {
    takenBefore?: string;
    takenAfter?: string;
  }
</script>

<script lang="ts">
  import DateInput from '$lib/components/elements/date-input.svelte';

  export let filters: SearchDateFilter;
</script>

<div id="date-range-selection" class="grid grid-cols-[repeat(auto-fit,minmax(10rem,1fr))] gap-5">
  <label class="immich-form-label" for="start-date">
    <span>开始时间</span>
    <DateInput
      class="immich-form-input w-full mt-1 hover:cursor-pointer"
      type="date"
      id="start-date"
      name="start-date"
      max={filters.takenBefore}
      bind:value={filters.takenAfter}
    />
  </label>

  <label class="immich-form-label" for="end-date">
    <span>结束时间</span>
    <DateInput
      class="immich-form-input w-full mt-1 hover:cursor-pointer"
      type="date"
      id="end-date"
      name="end-date"
      placeholder=""
      min={filters.takenAfter}
      bind:value={filters.takenBefore}
    />
  </label>
</div>
