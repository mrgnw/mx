<script>
		// todo: flip base/conversion
		// todo: chance currencies
		// bonus: calculator (accept '208*4')
		import { onMount } from "svelte";

		let base_unit = "mxn";

		let rate = 0.048;
		export let value;
		let examples = [100, 200, 400, 500, 1000, 2000, 5000];

		onMount(async () => {
		  const res = await fetch(`https://open.er-api.com/v6/latest/${base_unit}`);
		  let data = await res.json();
		  rate = data.rates.USD;
		  console.log(rate);
		});

		function format_num(num) {
		  if (num >= 1000) {
		    return `${num.toFixed(2) / 1000}k`;
		  } else {
		    return num.toLocaleString("en-US");
		  }
		}
</script>

<svelte:head>
	<title>mxn-usd</title>
</svelte:head>

<div class="foundation">
  <div class="div1">
  
    <input bind:value inputmode="numeric" placeholder="mxn" />
  </div>
  <div class="output div2">
    {#if (rate*value) || 0 > 0}
    <span class="converted-rate">
    ${((rate*value) || 0).toFixed(2) }</span> usd
    {/if}
  </div> <!-- output -->


  <div class="samples  div3">
  <!-- pivot table if window is wider? -->
  <table>
  <tr>
    <th>{rate.toFixed(4)} mxn</th>
    <th>▸</th>
    <th class="dim">$1 usd</th>
  </tr>
  {#each examples as ex}
  <tr>
    <td>{ex}</td>
    <td></td>
    <td>${(ex*rate).toFixed(0)}</td>
  </tr>
  {/each}
  </table>
  </div>

</div>
<style>
  :root {
    --blue: #0096ff;
    --green: #2ce69e;
  }
  .foundation {
    max-width: 100%;
    display: grid;
    grid-template-columns: 100%;
    grid-template-rows: 5em 5em auto;
    grid-column-gap: 0px;
    /* grid-row-gap: 20px; */
    height: 100% !important;
    min-height: 100% !important;
  }
  .div1,
  .output,
  .samples {
    width: 100%;
    max-width: 100%;
  }
  .div1 {
    z-index: 99;
  }
  .samples {
    opacity: 50%;
    overflow: scroll;
    height: 100%;
  }
  .output {
    text-align: center;
  }

  .dim {
    color: black;
  }

  input {
    width: 100%;
    font-size: 4em;
    color: var(--green);
    text-align: center;
  }

  .output {
    color: var(--blue);
    font-size: 4em;
  }

  @media only screen and (max-width: 767px) {
    .div1 {
      grid-row-end: -1;
    }
    .output {
      grid-row-end: -2;
    }
    .foundation {
      grid-template-rows: auto 5em 5em;
    }
  }

  * {
    font-family: monospace;
  }
  td {
    font-size: 1.5em;
  }

  th {
    font-size: 2em;
  }

  td:nth-child(1),
  th:nth-child(1) {
    color: var(--green);
    text-align: right;
  }

  th:nth-child(3) {
    color: var(--blue);
    text-align: left;
  }

  td:nth-child(3) {
    color: var(--blue);
  }

  table {
    margin: auto;
    font-size: 2em;
  }
</style>


