<script>
  import { onMount } from 'svelte'
  import ResizeChecker from './ResizeChecker.svelte'
  import GridItem from './GridItem.svelte'

  export let data
  let component
  let hasListener
  let timeout
  let columns = []
  let breakpoints = [800]
  let breakpoint = 0
  const columnData = [data.slice(3), data]

  const options = {
    root: null,
    rootMargin: "0px",
    threshold: [0]
  }
  
  const observer = new IntersectionObserver(handleIntersect, options)
  
  onMount(() => {
    observer.observe(component)
  })

  function handleIntersect(entries) {
    entries.forEach((entry) => {      
      const { isIntersecting } = entry

      if(isIntersecting && !hasListener) {
        hasListener = true
        document.addEventListener('scroll', scrollHandler, false)
      } 
      if(!isIntersecting && hasListener) {
        hasListener = false
        document.removeEventListener('scroll', scrollHandler, false)
      }
    })
  }

  function scrollHandler() {
    debounce(() => {
      const y = (window.innerHeight - component.getBoundingClientRect().top) * .01
      
      columns.forEach((column, index) => {
        if(column) {
          const { speed } = data[index]

          column.animate({
            transform: `translateY(${y * speed}px)`,
            easing: 'ease-out'
          }, {
            duration: 500,
            fill: 'both'
          })
        }
      })  
    })
  }

  function debounce(fn) {
    if (timeout) {
      window.cancelAnimationFrame(timeout);
    }
    timeout = window.requestAnimationFrame(() => fn())
  }
</script>

<ResizeChecker bind:breakpoint={breakpoint} breakpoints={breakpoints}/>
<article class="component" bind:this={component}>
  <div class="grid">
    {#each columnData[breakpoint] as column, index}
      <ul class="column" bind:this={columns[index]}>
        {#each column.images as item}
          <li class="cell" style={`--ratio: ${item.height / item.width * 100}%`}>
            <GridItem data={item} />
          </li>
        {/each}
      </ul>
    {/each}
  </div>
</article>


<style>
  .component {
    --gap: 10px;
    --radius: 10px;
    --blender: 150px;

    position: relative;
    z-index: 10;
    margin-top: var(--blender);

    background-color: white;
    transform: translateZ(0); /* force gpu to avoid render glitches */
  }

  .component:before {
    content: '';
    position: absolute;
    bottom: 100%;
    left: 0;
    width: 100%;
    height: var(--blender);
    background-image: linear-gradient(to bottom, rgba(255, 255, 255, 0), white);
    background-size: cover;
    pointer-events: none;
  }

  .grid {
    width: 100%;
    display: grid;
    grid-template-columns: .5fr 1fr 1fr .5fr;
    grid-gap: var(--gap);
  }

  .column {
    display: grid;
    align-content: center;
    list-style: none;

    overflow: hidden;
    will-change: transform;
  }

  .column:first-child li,
  .column:last-child li {
    width: 200%;
  }

  .column:first-child li {
    transform: translateX(-50%);
  }

  .cell {
    position: relative;
  }

  .cell:before {
    content: '';
    display: block;
    padding-top: var(--ratio);
  }

  .cell + .cell {
    margin-top: var(--gap);
  }

  @media screen and (min-width: 800px) {
    .component {
      --gap: var(--unit-medium);
      --radius: 20px;
      --blender: 200px;
    }

    .grid {
      grid-template-columns: .5fr 1fr 1fr 1fr 1fr 1fr .5fr;
    }
  }

  @media screen and (min-width: 1200px) {
    .component {
      --blender: 250px;
    }
  }
</style>