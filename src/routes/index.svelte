<script>
  import { onMount } from "svelte";
  const dt = new Date();
  const start = `${dt.getFullYear()}-0${dt.getMonth() +
    1}-${dt.getDate()}T00:00:00Z`;
  let data = [];
  let visible = false;

  const dataurl = `https://api.covid19india.org/raw_data.json`;
  onMount(async () => {
    const res = await fetch(dataurl);
    const json = await res.json();
    data = json.raw_data
      .filter(d => d.dateannounced)
      .filter(d => d.notes)
      .filter(d => d.agebracket)
      .filter(d => d.notes !== "Details awaited")
      .filter(d => d.notes !== "Details Awaited")
      .sort((a, b) => Number(b.patientnumber) - Number(a.patientnumber));
    visible = true;
  });
</script>

<style>
  /* Stat */
  .stat {
    display: inline-flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .stat h3 {
    margin: 0;
    font-size: 3rem;
    font-weight: 100;
  }

  .stat p {
    margin: 0;
    font-weight: 100;
  }
</style>

<svelte:head>
  <title>COVID-19 India</title>
</svelte:head>

<div class="jumbotron text-center">
  <h2 class="display-4">COVID-19</h2>
  <h4>How it's spreading</h4>
  <span class="small">Missing patient number = Details awaited.</span>
</div>
{#if visible}
{#each data as pat}
<div class="card shadow my-2 border-0">
  <div class="card-body">
  <p>#P{pat.patientnumber} : {pat.notes}</p>
  </div>
</div>
{/each}
{:else}
<div class="card shadow my-2 border-0">
  <div class="card-body text-center">
  <p>Loading patient data...</p>
  <div class="spinner-border" role="status">
  <span class="sr-only">Loading...</span>
</div>
  </div>
</div>

{/if}
<!-- <div class="card border-0 my-2 bg-transparent">
  <div class="card-body p-0">
    <div
      class="d-flex flex-row align-content-stretch flex-wrap
      justify-content-between">
      <div class="stat p-2">
        <h3>+{newConfirmed}</h3>
        <p>{totalConfirmed}</p>
        <p>Confirmed</p>
      </div>
      <div class="stat p-2">
        <h3>+{newDeaths}</h3>
        <p>{totalDeaths}</p>
        <p>Deaths</p>
      </div>
      <div class="stat p-2">
        <h3>+{newRecovered}</h3>
        <p>{totalRecovered}</p>
        <p>Recovered</p>
      </div>
    </div>
  </div>
</div> -->
<div class="py-5 bg-light text-muted">
  <div class="container-fluid">
        <div class="font-weight-bold text-uppercase text-lg text-dark mb-3">
          COVID-19
        </div>
        <p>
          Fun project to play with
          <a href="https://covid19api.com/" target="_blank" rel="noopener">
            api
          </a>
        </p>
        <p>
          Developed By
          <a
            href="https://xpressabhi.github.io/"
            target="_blank"
            rel="noopener">
            Abhishek Maurya
          </a>
        </p>
  </div>
</div>
