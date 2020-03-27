<script>
  import { onMount } from "svelte";
  const dt = new Date();
  const start = `${dt.getFullYear()}-0${dt.getMonth() +
    1}-${dt.getDate()}T00:00:00Z`;
  let data;

  let totalConfirmed = 0;
  let newConfirmed = 0;
  const confirmedurl = `https://api.covid19api.com/live/country/india/status/confirmed/date/${start}`;
  onMount(async () => {
    const res = await fetch(confirmedurl);
    const json = await res.json();
    data = json.sort((a, b) => new Date(b.Date) - new Date(a.Date));
    newConfirmed = Number(data[0].Cases) - Number(data[data.length - 1].Cases);
    totalConfirmed = data[0].Cases;
  });
  let totalDeaths = 0;
  let newDeaths = 0;
  const deathsurl = `https://api.covid19api.com/live/country/india/status/deaths/date/${start}`;
  onMount(async () => {
    const res = await fetch(deathsurl);
    const json = await res.json();
    data = json.sort((a, b) => new Date(b.Date) - new Date(a.Date));
    newDeaths = Number(data[0].Cases) - Number(data[data.length - 1].Cases);
    totalDeaths = data[0].Cases;
  });
  let totalRecovered = 0;
  let newRecovered = 0;
  const recoveredurl = `https://api.covid19api.com/live/country/india/status/recovered/date/${start}`;
  onMount(async () => {
    const res = await fetch(recoveredurl);
    const json = await res.json();
    data = json.sort((a, b) => new Date(b.Date) - new Date(a.Date));
    newRecovered = Number(data[0].Cases) - Number(data[data.length - 1].Cases);
    totalRecovered = data[0].Cases;
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

  .stat h3,
  .stat h4 {
    margin: 0;
    font-size: 4rem;
    font-weight: 100;
  }

  .stat p {
    margin: 0;
    font-weight: 100;
  }

  .stat.flex-row p {
    padding-left: 0.5rem;
  }
</style>

<svelte:head>
  <title>COVID-19 India</title>
</svelte:head>

<div class="jumbotron">
  <h2 class="display-4">COVID-19, India</h2>
  <span class="small text-muted">+ = Added today</span>
</div>

<div class="card border-0 my-2 bg-transparent">
  <div class="card-body p-0">
    <div
      class="d-flex flex-row align-content-stretch flex-wrap
      justify-content-between">
      <div class="stat p-2">
        <h3>+{newConfirmed}</h3>
        <p>Confirmed {totalConfirmed}</p>
      </div>
      <div class="stat p-2">
        <h3>+{newDeaths}</h3>
        <p>Deaths {totalDeaths}</p>
      </div>
      <div class="stat p-2">
        <h3>+{newRecovered}</h3>
        <p>Recovered {totalRecovered}</p>
      </div>
    </div>
  </div>
</div>
<div class="py-5 bg-light text-muted">
  <br />
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-8">
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
      <div class="col-md-4">
        <img
          src="https://res.cloudinary.com/xpsabhi/image/upload/v1547488584/undraw_contact_us_15o2.svg"
          class="img-fluid"
          alt="" />
      </div>
    </div>
  </div>
  <br />
</div>
