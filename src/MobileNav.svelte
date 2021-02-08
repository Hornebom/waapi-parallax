<script>
  import { onMount } from 'svelte'

  let component
  let items = []
  let cheese, burger
  let initial = true
  const duration = 350
  const stagger = 50
  const animations = []
  const easing = 'cubic-bezier(.17,.67,.35,.98)'

  const frames = {
    transform: ['translateY(100%)', 'translateY(0%)'],
    opacity: [0, 1],
    offset: 0
  }

  const options = {
    duration,
    fill: 'both',
    easing
  }

  onMount(() => {
    animations.push(
      component.animate(frames, options)
    )

    items.forEach((item, index) => {
      animations.push(
        item.animate(frames, {
          ...options,
          delay: duration * .5 + index * stagger,
        })
      )
    })

    animations.push(
      cheese.animate({
        transform: ['rotate(0deg)', 'rotate(45deg)'],
        boxShadow: '0 0 0 white'
      }, options)
    )

    animations.push(
      burger.animate({
        transform: ['rotate(0deg)', 'rotate(135deg)'],
      }, options)
    )

    animations.forEach(animation => animation.pause())
  })

  function handleToggle(event) {
    event.preventDefault()

    if(initial) {
      initial = false
      animations.forEach(animation => animation.play())
    } else {
      animations.forEach(animation => animation.reverse())
    }
  }
</script>

<nav class="component" bind:this={component}>
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

<a href="/" class="toggle" on:click={handleToggle}>
  <span class="cheese" bind:this={cheese}/>
  <span class="burger" bind:this={burger}/>
  <span class="sr-only">Toggle Menu</span>
</a>

<style>
  .component {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: var(--unit-xlarge) var(--unit-medium);
    
    background-color: var(--color-primary);

    transform: translateY(100%);
    will-change: transform;
  }

  .items {
    list-style: none;
  }

  .item + .item {
    margin-top: 10px;
  }

  .link {
    display: block;
    text-align: center;
    font-size: 2.2rem;
    font-family: var(--font-bold);
    text-decoration: none;
    color: var(--color-text-light);
  }

  .toggle {
    position: fixed;
    right: var(--unit-medium);
    bottom: var(--unit-medium);
    width: 50px;
    height: 50px;
    
    border-radius: 50%;
    background-color: var(--color-primary);
    box-shadow: 0 0 30px  rgba(0, 0, 0, .25);
  }

  .cheese,
  .burger {
    content: '';
    position: absolute;
    top: calc(50% - 1px);
    left: calc(50% - 15px);
    width: 30px;
    height: 2px;

    background-color: white;

    will-change: transform;
  }

  .cheese {
    box-shadow: 0 -8px 0 white, 0 8px 0 white
  }
</style>