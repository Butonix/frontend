<div
  class="{isDesktopVersion && isFixedDesktopHeader ? 'header-fixed' : ''} layout"
>
  {#if isDesktopVersion}
    <DesktopHeader />
  {:else}
    <MobileMenu />
  {/if}

  <slot />

  <Footer />
</div>

<script>
  import { onMount } from 'svelte';
  import { stores } from '@sapper/app';

  import DesktopHeader from 'components/layout/main/desktop-header.svelte';
  import MobileMenu from 'components/layout/main/mobile-menu.svelte';
  import Footer from 'components/layout/main/footer.svelte';

  let { page } = stores();

  $: isFixedDesktopHeader = $page.params && !$page.params.articleId;

  let isDesktopVersion = true;

  onMount(() => {
    /* Determine which device the application is open on */
    let media = window.matchMedia('(min-width: 941px)');

    isDesktopVersion = media.matches;

    function changeVersion(e) {
      isDesktopVersion = e.matches;
    }

    media.addEventListener('change', changeVersion);

    return () => {
      media.removeEventListener('change', changeVersion);
    };
  });
</script>

<style global>
  @import '../styles/common.css';

  .layout {
    display: grid;
    grid-template-areas:
      'header'
      'main'
      'footer';
    grid-template-rows: auto 1fr auto;
    grid-template-columns: auto;
    height: 100%;
  }

  .layout > main {
    grid-area: main;
  }

  .layout.header-fixed {
    padding-top: 65px;
  }

  .layout.header-fixed > header {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
  }
</style>
