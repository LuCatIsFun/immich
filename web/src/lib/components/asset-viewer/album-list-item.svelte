<script lang="ts">
  import { getAssetThumbnailUrl } from '$lib/utils';
  import { ThumbnailFormat, type AlbumResponseDto } from '@immich/sdk';
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher<{
    album: void;
  }>();

  export let album: AlbumResponseDto;
  export let variant: 'simple' | 'full' = 'full';
  export let searchQuery = '';
  let albumNameArray: string[] = ['', '', ''];

  // This part of the code is responsible for splitting album name into 3 parts where part 2 is the search query
  // It is used to highlight the search query in the album name
  $: {
    let { albumName } = album;
    let findIndex = albumName.toLowerCase().indexOf(searchQuery.toLowerCase());
    let findLength = searchQuery.length;
    albumNameArray = [
      albumName.slice(0, findIndex),
      albumName.slice(findIndex, findIndex + findLength),
      albumName.slice(findIndex + findLength),
    ];
  }
</script>

<button
  on:click={() => dispatch('album')}
  class="flex w-full gap-4 px-6 py-2 text-left transition-colors hover:bg-gray-200 dark:hover:bg-gray-700"
>
  <div class="h-12 w-12 shrink-0 rounded-xl bg-slate-300">
    {#if album.albumThumbnailAssetId}
      <img
        src={getAssetThumbnailUrl(album.albumThumbnailAssetId, ThumbnailFormat.Webp)}
        alt={album.albumName}
        class="z-0 h-full w-full rounded-xl object-cover transition-all duration-300 hover:shadow-lg"
        data-testid="album-image"
        draggable="false"
      />
    {/if}
  </div>
  <div class="flex h-12 flex-col items-start justify-center overflow-hidden">
    <span class="w-full shrink overflow-hidden text-ellipsis whitespace-nowrap"
      >{albumNameArray[0]}<b>{albumNameArray[1]}</b>{albumNameArray[2]}</span
    >
    <span class="flex gap-1 text-sm">
      {#if variant === 'simple'}
        <span>{album.shared ? 'Shared' : ''}</span>
      {:else}
        <span>{album.assetCount} 项</span>
        <span>{album.shared ? ' · Shared' : ''} </span>
      {/if}
    </span>
  </div>
</button>
