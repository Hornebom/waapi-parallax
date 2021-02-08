<script>
  import data from './data.js'

  import Header from './Header.svelte'
  import Hero from './Hero.svelte'
  import Grid from './Grid.svelte'
  import Usps from './Usps.svelte'
  import Footer from './Footer.svelte'
</script>

<Header />
<main>
  <div>
    <Hero />
    <Grid data={data.grid} />
  </div>
  <Usps data={data.usps} />
</main>
<Footer />
