<script>
	export let name;

	/* onMount(() => {
        console.log("the component has mounted");
        Plotly.newPlot("myDiv", data, layout)
    }); */

    // http://192.168.1.13:8001/?A1=66&A2=75&A3=18&A4=54&B1=54&B2=23&B3=68&B4=35&C1=62&C2=19&C3=79&C4=98&D1=30&D2=52&D3=86&D4=34&
	function drawGraph() {

        Plotly.newPlot("myDiv", data, layout)
	}

    let allAxis = []
    let values = []
    let data = []
    let layout = {}

    const currUrl = window.location.search
    const urlParams = new URLSearchParams(currUrl);
    let entries = urlParams.entries();

    for(let [key, value] of entries) {
        console.log(key + " => " + value)
        allAxis.push(key)
        values.push(value)
    }

    console.log("all axis: " + allAxis)
    console.log("all values: " + values)
    

    data = [
      {
        type: 'scatterpolar',
        r: values,
        theta: allAxis,
        fill: 'toself',
        name: 'Group A'
      },
      
    ]

    layout = {
      polar: {
        radialaxis: {
          visible: true,
          range: [0, 100]
        }
      }
    }



</script>

<main>
	<h1>Hello {name}!</h1>
	<div>
		<button on:click="{drawGraph}">Draw graph</button>
	</div>
	<div id="myDiv"></div>


</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>