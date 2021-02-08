<script>
  import ResizeChecker from './ResizeChecker.svelte'
  import MobileNav from './MobileNav.svelte'
  import DesktopNav from './DesktopNav.svelte'

  let breakpoints = [900]
  let breakpoint = 0
</script>

<ResizeChecker bind:breakpoint={breakpoint} breakpoints={breakpoints}/>

<header class="component">
  <div class="inner">
    <a href="/" class="logo">ogol</a>

    {#if breakpoint === 0}
      <MobileNav />
    {:else if breakpoint === 1 }
      <DesktopNav className="desktop-nav" />
    {/if}
  </div>
</header>

<style>
  .component {
    position: relative;
    z-index: 100;
    height: 0;
    overflow: visible;
  }

  .inner {
    max-width: 1400px;
    margin-right: auto;
    margin-left: auto;
    display: flex;
    padding: var(--unit-medium);
  }

  .logo {
    width: 150px;
    font-size: 2.4rem;
    font-family: var(--font-bold);
    color: var(--color-primary);
    text-decoration: none;
  }

  :global(.desktop-nav) {
    margin-left: auto;
  }  

  @media screen and (min-width: 900px) {
    .inner {
      padding: var(--unit-large);
    }
  }
</style>