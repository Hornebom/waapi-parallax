<script>
  import { onMount } from 'svelte'
  
  export let data
  
  let component
  let initial = true
  let items = []
  let figures = []
  let captions = []
  let animations = []
  const stagger = 40
  const delay = 100

  const options = {
    root: null,
    rootMargin: "0px 0px -200px 0px",
    threshold: [0]
  }

  const timing = {
    duration: 300,
    fill: 'both',
    easing: 'cubic-bezier(.17,.67,.35,.98)'
  }
  
  const observer = new IntersectionObserver(handleIntersect, options)

  onMount(() => {
    items.forEach((item, index) => {
      animations.push(
        item.animate({
          transform: ['translateY(80px)', 'translateY(0px)'],
          opacity: [0, 1]
        }, {
          delay: stagger * index,
          ...timing
        })
      )
    })

    figures.forEach((figure, index) => {
      animations.push(
        figure.animate({
          transform: ['translateY(50%) scaleX(.3)', 'translateY(0%) scaleX(1)'],
          opacity: [0, 1]
        }, {
          delay: stagger * index,
          ...timing
        })
      )
    })

    captions.forEach((caption, index) => {
      animations.push(
        caption.animate({
          transform: ['translateY(80px)', 'translateY(0px)'],
          opacity: [0, 1]
        }, {
          delay: delay + stagger * index,
          ...timing
        })
      )
    })

    animations.forEach(animation => animation.pause())
    observer.observe(component)
  })

  function handleIntersect(entries) {
    entries.forEach((entry) => {      
      const { isIntersecting } = entry
      if(isIntersecting && initial) {
        initial = false
        animations.forEach(animation => animation.play())
      } 
    })
  }

</script>

<article class="component" bind:this={component}>
  <h2 class="h3" bind:this={items[0]}>Discover</h2>
  <p class="body" bind:this={items[1]}>
    Our smart search engine allows you to find answers to any question. Search topics, insigts or humans to find exactly what you are looking for in no time.
  </p>
  <ol class="items">
    {#each data as item, index}
      <li class="item">
        <div class="caption" bind:this={captions[index]}>
          <h3 class="h4 title">{ item.title }</h3>
          <p>{ item.body }</p>
        </div>
        <figure class="media" bind:this={figures[index]}>
          <img
            src={`/images/${item.path}.svg`}
            loading="lazy"
            alt />
        </figure>
      </li>
    {/each}
  </ol>
</article>

<style>
  .component {
    position: relative;
    z-index: 2;
    padding-right: var(--unit-medium);
    padding-left: var(--unit-medium);
    
    text-align: center;
    background-color: white;
  }

  .body {
    max-width: var(--max-width-small);
    margin-top: var(--unit-medium);
    margin-right: auto;
    margin-left: auto;
  }

  .items {
    max-width: var(--max-width-large);
    margin-top: var(--unit-xlarge);
    margin-right: auto;
    margin-left: auto;
    
    list-style: none;
  }

  .item + .item {
    margin-top: var(--unit-large);
  }

  .item {
    display: flex;
    flex-direction: column;

    max-width: 400px;
    margin-right: auto;
    margin-left: auto;
  }

  .caption {
    order: 2;

    margin-top: var(--unit-medium);
  }

  .media {
    order: 1;
    position: relative;

    background-color: var(--color-primary);
    border-radius: 20px;
    overflow: hidden;

    transform-origin: 50% 100%;
    will-change: transform;
  }

  .media:before {
    content: '';
    display: block;
    padding-top: 100%;
  }

  .media img {
    --unit: 16%;
    position: absolute;
    top: var(--unit);
    left: var(--unit);
    width: calc(100% - var(--unit) * 2);
    height: auto;
  }

  @media screen and (min-width: 800px) {
    .items {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-gap: var(--unit-medium);
    }

    .item + .item {
      margin-top: 0;
    }

    .item {
      max-width: none;
      margin-right: initial;
      margin-left: initial;
    }

    .media {
      border-radius: 50px;
    }
  }
</style>