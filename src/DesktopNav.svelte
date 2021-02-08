<script>
  import { onMount } from 'svelte'
  
  export let className = ''
  let items = []

  onMount(() => {
    items.forEach((item, index) => {
      item.animate({
        opacity: [0, 1],
        easing: 'linear'
      }, {
        duration: 400,
        delay: 100 + 60 * index,
        fill: 'both'
      })
    })
  })
</script>

<nav class={`component ${className}`}>
  <ul class="items">
    <li class="item" bind:this={items[0]}>
      <a href="/" class="link">Discover</a>
    </li>
    <li class="item" bind:this={items[1]}>
      <a href="/" class="link">Articles</a>
    </li>
    <li class="item" bind:this={items[2]}>
      <a href="/" class="link">Help</a>
    </li>
    <li class="item" bind:this={items[3]}>
      <a href="/" class="link cta">About ogol</a>
    </li>
  </ul>
</nav>

<style>
  .component {
    
  }

  .items {
    display: flex;
    justify-content: flex-end;

    list-style: none;
  }

  .item + .item {
    margin-left: 50px;
  }

  .link {
    font-size: 1.7rem;
    color: var(--color-text-dark);
    text-decoration: none;
  }

  .cta {
    padding: 1em 2em;
    background-color: var(--color-primary);
    border-radius: 40px;
    color: var(--color-text-light);
    font-family: var(--font-bold);
  }
</style>