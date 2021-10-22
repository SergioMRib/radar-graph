<script>
import {onMount} from "svelte";
import { element } from "svelte/internal";


	export let name;
	let firstDraw = false;

	onMount(() => {
        console.log("the component has mounted");
        Plotly.newPlot("myDiv", data, layout)
    });

    // http://192.168.1.13:8001/?A1=66&A2=75&A3=18&A4=54&B1=54&B2=23&B3=68&B4=35&C1=62&C2=19&C3=79&C4=98&D1=30&D2=52&D3=86&D4=34&
	function drawGraph() {

        Plotly.newPlot("myDiv", data, layout)
		firstDraw = true;
	}

	function drawRandGraph() {

		//Plotly.deleteTraces('myDiv', 0);
		values = values.map(element => {
			return Math.ceil(Math.random() * 50 + 50);
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
    	  		    range: [0, 100]
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
			return Math.ceil(Math.random() * 50 + 50);
		}); 

		let values2 = values.map(() => Math.ceil(Math.random() * 70 + 30));		
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
    	  		    range: [0, 100]
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
          range: [0, 100]
        }
      }
    }



</script>

<main>
	<h3 class="mb-5">Your data results</h3>
	<div>
		<button class="btn btn-primary" on:click="{drawGraph}">Redraw graph from query</button>
		
		
	</div>
	<div id="myDiv"></div>
	<button class="btn btn-danger mr-2" on:click="{drawRandGraph}">Draw a random graph</button>
	<button class="btn btn-warning ml-2" on:click="{drawTwoGraphs}">Draw two random graphs</button>

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