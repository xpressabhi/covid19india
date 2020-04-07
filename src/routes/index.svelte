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
  <p>#P{pat.patientnumber}[Age {pat.agebracket}] : {pat.notes}</p>
  <span class="small">Detected @ {pat.detectedcity} {pat.detecteddistrict} {pat.detectedstate}</span>
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
