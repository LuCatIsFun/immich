<script lang="ts">
  import { page } from '$app/stores';
  import SideBarButton from '$lib/components/shared-components/side-bar/side-bar-button.svelte';
  import SideBarSection from '$lib/components/shared-components/side-bar/side-bar-section.svelte';
  import StatusBox from '$lib/components/shared-components/status-box.svelte';
  import { AppRoute } from '$lib/constants';
  import { mdiAccountMultipleOutline, mdiBookshelf, mdiCog, mdiServer, mdiSync, mdiTools } from '@mdi/js';
</script>

<SideBarSection>
  <a data-sveltekit-preload-data="hover" href={AppRoute.ADMIN_USER_MANAGEMENT} draggable="false">
    <SideBarButton
      title="用户"
      icon={mdiAccountMultipleOutline}
      isSelected={$page.route.id === AppRoute.ADMIN_USER_MANAGEMENT}
    />
  </a>
  <a data-sveltekit-preload-data="hover" href={AppRoute.ADMIN_JOBS} draggable="false">
    <SideBarButton title="任务" icon={mdiSync} isSelected={$page.route.id === AppRoute.ADMIN_JOBS} />
  </a>
  <a data-sveltekit-preload-data="hover" href={AppRoute.ADMIN_SETTINGS} draggable="false">
    <SideBarButton title="设置" icon={mdiCog} isSelected={$page.route.id === AppRoute.ADMIN_SETTINGS} />
  </a>
  <a data-sveltekit-preload-data="hover" href={AppRoute.ADMIN_LIBRARY_MANAGEMENT} draggable="false">
    <SideBarButton
      title="外部库"
      icon={mdiBookshelf}
      isSelected={$page.route.id === AppRoute.ADMIN_LIBRARY_MANAGEMENT}
    />
  </a>
  <a data-sveltekit-preload-data="hover" href={AppRoute.ADMIN_STATS} draggable="false">
    <SideBarButton title="状态" icon={mdiServer} isSelected={$page.route.id === AppRoute.ADMIN_STATS} />
  </a>
  <a data-sveltekit-preload-data="off" href={AppRoute.ADMIN_REPAIR} draggable="false">
    <SideBarButton title="修复" icon={mdiTools} isSelected={$page.route.id === AppRoute.ADMIN_REPAIR} />
  </a>
  <div class="mb-6 mt-auto">
    <StatusBox />
  </div>
</SideBarSection>
