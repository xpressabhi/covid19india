<script>
	import { onMount } from "svelte";
	let data = [];
	let date;
	onMount(async () => {
	  const res = await fetch(
	    "https://api.covid19api.com/live/country/india/status/confirmed"
	  );
	  const json = await res.json();
	  console.log(json);
	  data = json.sort((a, b) => {
	    return new Date(b.Date) - new Date(a.Date);
	  });
	  console.log(data);
	});
</script>
<style>
  table,
  th,
  td {
    border: 1px solid black;
  }
</style>
<svelte:head>
	<title>COVID-19 India</title>
</svelte:head>

<h1>Indian Confirmed Cases</h1>
<table class="table">
  <thead>
    <tr>
      <th scope="col">Date</th>
			<th scope="col">Cases</th>
    </tr>
  </thead>
	{#each data as d}
  <tbody>
    <tr>
      <td>{d.Date}</td>
      <td>{d.Cases}</td>
    </tr>
  </tbody>
	{/each}
</table>
