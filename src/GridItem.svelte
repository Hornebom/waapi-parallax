<script>
  import { onMount } from 'svelte'

  export let data
  let component

  onMount(() => {
    component.animate({
      transform: ['scale(.5)', 'scale(1)'],
      opacity: [0, 1],
      easing: 'cubic-bezier(.13,.5,.43,.94)'
    }, {
      duration: 300,
      delay: Math.random() * 200,
      fill: 'both'
    })
  })
</script>


<picture>
  <source
    type="image/webp"
    srcset="/images/{data.path}.webp"
  />
  <img
    class="component"
    src="/images/{data.path}.jpg"
    width={data.width}
    height={data.height}
    loading="lazy"
    alt
    bind:this={component}
  />
</picture>

<style>
  .component {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    
    border-radius: var(--radius);
    overflow: hidden;

    will-change: transform;
  }
</style>