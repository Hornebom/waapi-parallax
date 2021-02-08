<script>
  import { onMount } from 'svelte'

  let items = []
  const timing = {
    duration: 400,
    fill: 'both'
  }
  const stagger = 40
  const delay = 200

  onMount(() => {
    items.forEach((item, index) => {
      item.animate({
        transform: ['translateY(50px)', 'translateY(0px)'],
        easing: 'ease-out',
        offset: 0
      }, {
        ...timing,
        delay: delay + stagger * index,
      })
      item.animate({
        opacity: [0, 1],
        easing: 'linear',
        offset: 0
      }, {
        ...timing,
        delay: delay + stagger * index,
      })
    })

  })
</script>

<article class="component">
  <h2 class="title">
    <span class="isAnimated" bind:this={items[0]}>Social learning</span>
    <br/>
    <span class="isAnimated" bind:this={items[1]}>network</span>
  </h2>
  <p class="body isAnimated" bind:this={items[2]}>
    Discover the beauty in learning, meet interesting humans, and share insights. A social learning network build to inspire growth worldwide.
  </p>
  <nav class="nav">
    <ul class="items">
      <li class="item isAnimated" bind:this={items[3]}>
        <a href="/" class="link">
          <span class="sr-only">Play store</span>
          <img src="/images/btn-google-play-high-res.png" alt="">
        </a>
      </li>
      <li class="item isAnimated" bind:this={items[4]}>
        <a href="/" class="link">
          <span class="sr-only">App store</span>
          <img src="/images/btn-app-store.svg" alt="">
        </a>
      </li>
    </ul>
  </nav>
</article>

<style>
  .component {
    position: sticky;
    top: 0;
    z-index: 1;
    padding-top: 10rem;
    
    text-align: center;
  }

  .isAnimated {
    opacity: 0;
    will-change: transform;
  }

  .title {
    font-family: var(--font-bold);
    font-size: 9vw;
    line-height: 1.1em;
  }

  .title span {
    display: inline-block;
  }

  .body {
    max-width: var(--max-width-small);
    margin-top: var(--unit-medium);
    margin-right: auto;
    margin-left: auto;
    padding-right: var(--unit-medium);
    padding-left: var(--unit-medium);
  }

  .nav {
    margin-top: var(--unit-large);
  }

  .items {
    --width: 120px;
    --height: 36px;
    
    display: grid;
    grid-template-columns: repeat(2, var(--width));
    grid-gap: var(--unit-medium);
    justify-content: center;
    list-style: none;
  }

  .item {
    height: var(--height);
  }

  .link {
    position: relative;
    display: block;
    height: 100%;
  }

  .link img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  @media screen and (min-width: 800px) {
    .component {
      padding-top: 14rem;
    }

    .title {
      font-size: 7vw;
    }

    .nav {
      margin-top: var(--unit-xlarge);
    }

    .items {
      --width: 160px;
      --height: 48px;
    }
  }

  @media screen and (min-width: 1200px) {
    .component {
      padding-top: 18rem;
    }

    .title {
      font-size: 9rem;
    }
  }
</style>