<script>
import {onMount} from "svelte";
import { element } from "svelte/internal";
import Chart from 'chart.js/auto';



	export let name;
	let seeDinamics = false;

	onMount(() => {
        console.log("the component has mounted");
        Plotly.newPlot("myDiv", data, layout)
    });

    // http://192.168.1.13:8001/?A1=66&A2=75&A3=18&A4=54&B1=54&B2=23&B3=68&B4=35&C1=62&C2=19&C3=79&C4=98&D1=30&D2=52&D3=86&D4=34&
	
	function toggleDinamics() {
		seeDinamics = !seeDinamics;
	}

	function drawGraph() {

        Plotly.newPlot("myDiv", data, layout)
		
	}

	function drawRandGraph() {

		//Plotly.deleteTraces('myDiv', 0);
		values = values.map(element => {
			return Math.ceil(Math.random() * 8 );
		}); 
		
		let data = [
      			{
        			type: 'scatterpolar',
        			r: values,
        			theta: allAxis,
        			fill: 'toself',
        			name: 'Group A'
      			},
    	]

    	let layout = {
      		polar: {
				radialaxis: {
    	  		    visible: true,
    	  		    range: [0, 8]
    	        }
    		}
    	}
		console.log("New values: " + data[0].r);
		console.log("Drawing graph")
        Plotly.newPlot("myDiv", data, layout)

	}

	function drawTwoGraphs() {

		//Plotly.deleteTraces('myDiv', 0);
		let values1 = values.map(element => {
			return Math.ceil(Math.random() * 8);
		}); 

		let values2 = values.map(() => Math.ceil(Math.random() * 6));		
		let data = [
      			{
        			type: 'scatterpolar',
        			r: values1,
        			theta: allAxis,
        			fill: 'toself',
        			name: 'Group A'
      			},
				  {
					  type: 'scatterpolar',
					  r: values2,
					  theta: allAxis,
					  fill: 'toself',
					  name: 'Group B'
				  }
    	]

    	let layout = {
      		polar: {
				radialaxis: {
    	  		    visible: true,
    	  		    range: [0, 8]
    	        }
    		}
    	}
		console.log("New values: " + data[0].r);
		console.log("Drawing graph")
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
          range: [0, 8]
        }
      }
    }



</script>

<main>
	<h3 class="mb-5">Your data results</h3>
	
	<div id="myDiv"></div>

	{#if seeDinamics}
		<div>
			<button class="btn btn-danger mb-3" on:click="{drawRandGraph}">Draw a random graph</button>
		</div>
		<div>
			<button class="btn btn-warning mb-3" on:click="{drawTwoGraphs}">Draw two random graphs</button>
		</div>
		<div>
			<button class="btn btn-primary mb-3" on:click="{drawGraph}">Redraw graph from query</button>
		</div>
	{/if}

	<div>
		<button class="btn btn-secondary mt-5" on:click="{toggleDinamics}">
			Try random values
		</button>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h3 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>