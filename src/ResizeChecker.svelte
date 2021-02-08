<script>
  import { onMount } from 'svelte'
  
  let component
  let timeout

  export let breakpoint
  export let breakpoints = []

  onMount(() => handleResize())

  function handleResize(event) {
    debounce(() => {
      [0, ...breakpoints].forEach((bp, index) => {
        if(window.innerWidth >= bp) {
          breakpoint = index
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

<svelte:window on:resize={handleResize}/>
