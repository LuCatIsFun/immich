<script lang="ts">
  import { goto } from '$app/navigation';
  import empty3Url from '$lib/assets/empty-3.svg';
  import LinkButton from '$lib/components/elements/buttons/link-button.svelte';
  import Icon from '$lib/components/elements/icon.svelte';
  import UserPageLayout from '$lib/components/layouts/user-page-layout.svelte';
  import DeleteAssets from '$lib/components/photos-page/actions/delete-assets.svelte';
  import RestoreAssets from '$lib/components/photos-page/actions/restore-assets.svelte';
  import SelectAllAssets from '$lib/components/photos-page/actions/select-all-assets.svelte';
  import AssetGrid from '$lib/components/photos-page/asset-grid.svelte';
  import AssetSelectControlBar from '$lib/components/photos-page/asset-select-control-bar.svelte';
  import ConfirmDialogue from '$lib/components/shared-components/confirm-dialogue.svelte';
  import EmptyPlaceholder from '$lib/components/shared-components/empty-placeholder.svelte';
  import {
    NotificationType,
    notificationController,
  } from '$lib/components/shared-components/notification/notification';
  import { AppRoute } from '$lib/constants';
  import { createAssetInteractionStore } from '$lib/stores/asset-interaction.store';
  import { AssetStore } from '$lib/stores/assets.store';
  import { featureFlags, serverConfig } from '$lib/stores/server-config.store';
  import { handleError } from '$lib/utils/handle-error';
  import { emptyTrash, restoreTrash } from '@immich/sdk';
  import { mdiDeleteOutline, mdiHistory } from '@mdi/js';
  import type { PageData } from './$types';
  import { handlePromiseError } from '$lib/utils';

  export let data: PageData;

  $featureFlags.trash || handlePromiseError(goto(AppRoute.PHOTOS));

  const assetStore = new AssetStore({ isTrashed: true });
  const assetInteractionStore = createAssetInteractionStore();
  const { isMultiSelectState, selectedAssets } = assetInteractionStore;
  let isShowEmptyConfirmation = false;

  const handleEmptyTrash = async () => {
    isShowEmptyConfirmation = false;
    try {
      await emptyTrash();

      notificationController.show({
        message: `Empty trash initiated. Refresh the page to see the changes`,
        type: NotificationType.Info,
      });
    } catch (error) {
      handleError(error, 'Error emptying trash');
    }
  };

  const handleRestoreTrash = async () => {
    try {
      await restoreTrash();

      notificationController.show({
        message: `Restore trash initiated. Refresh the page to see the changes`,
        type: NotificationType.Info,
      });
    } catch (error) {
      handleError(error, 'Error restoring trash');
    }
  };
</script>

{#if $isMultiSelectState}
  <AssetSelectControlBar assets={$selectedAssets} clearSelect={() => assetInteractionStore.clearMultiselect()}>
    <SelectAllAssets {assetStore} {assetInteractionStore} />
    <DeleteAssets force onAssetDelete={(assetIds) => assetStore.removeAssets(assetIds)} />
    <RestoreAssets onRestore={(assetIds) => assetStore.removeAssets(assetIds)} />
  </AssetSelectControlBar>
{/if}

{#if $featureFlags.loaded && $featureFlags.trash}
  <UserPageLayout hideNavbar={$isMultiSelectState} title={data.meta.title} scrollbar={false}>
    <div class="flex place-items-center gap-2" slot="buttons">
      <LinkButton on:click={handleRestoreTrash}>
        <div class="flex place-items-center gap-2 text-sm">
          <Icon path={mdiHistory} size="18" />
          恢复所有
        </div>
      </LinkButton>
      <LinkButton on:click={() => (isShowEmptyConfirmation = true)}>
        <div class="flex place-items-center gap-2 text-sm">
          <Icon path={mdiDeleteOutline} size="18" />
          清空回收站
        </div>
      </LinkButton>
    </div>

    <AssetGrid {assetStore} {assetInteractionStore}>
      <p class="font-medium text-gray-500/60 dark:text-gray-300/60 p-4">
        项目将会在 {$serverConfig.trashDays} 天后彻底被删除.
      </p>
      <EmptyPlaceholder
        text="删除的图片或视频将会暂存到这里，直至过期被彻底删除"
        alt="Empty trash can"
        slot="empty"
        src={empty3Url}
      />
    </AssetGrid>
  </UserPageLayout>
{/if}

{#if isShowEmptyConfirmation}
  <ConfirmDialogue
    title="清空回收站"
    confirmText="Empty"
    on:confirm={handleEmptyTrash}
    on:cancel={() => (isShowEmptyConfirmation = false)}
  >
    <svelte:fragment slot="prompt">
      <p>确认要清空回收站吗，这将彻底删除这些资源~！</p>
      <p><b>您无法撤销这个操作</b></p>
    </svelte:fragment>
  </ConfirmDialogue>
{/if}
