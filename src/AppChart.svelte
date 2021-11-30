<script>
import {onMount} from "svelte";
import { element } from "svelte/internal";
import Chart from 'chart.js/auto';

    let graphData = [];
    let canvas;
	let ctx;
    let myChart;
    let newValue = {
        key: '',
        value: ''
    }
    const currUrl = window.location.search;
    const urlParams = new URLSearchParams(currUrl);
    let entries = urlParams.entries();
    let maxWidth = 500;
    let editDetails = {
        info: false,
        data: false
    }
    
    let chartInfo = {
        label: "Total",
        borderWidth: 2,
        borderColor: "#3e95cd",
        backgroundColor: "rgb(62,149,205,0.1)",
    }

    for(let [key, value] of entries) {
       
        graphData.push({key: key, value: value})
    }

      
    function updateChartTitle(chart, label) {
        console.log(chart)
        chart.data.datasets[0].label = label;
        chart.update();
    }

    function updateChartValues(chart, data) {
        console.log(chart)
        console.log(data)
        chart.data.labels = data.map((record) => record.key)
        chart.data.datasets[0].data = data.map((record) => record.value)
        chart.update();
    }

    function addRecord(value, data, chart) {
        graphData = [...data, {key: value.key, value: value.value}]
        chart.data.labels.push(value.key)
        chart.data.datasets[0].data.push(value.value)
        newValue.key = ''
        newValue.value = ''
        chart.update();
    }

	function drawChart(info) {

	    canvas = document.getElementById("myChart")
        ctx = canvas.getContext('2d');		
        canvas.style.backgroundColor = "white" 
        //ctx.save();
        //ctx.fillStyle = 'blue';
        //ctx.fillRect(0, 0, canvas.width , canvas.height); 
       // ctx.restore()
        let myChart = new Chart(ctx, {
            type: 'radar',
            data: {
                labels: graphData.map((record) => record.key),
                datasets: [{ 
                    data: graphData.map((record) => record.value),  // the previous one was values
                    label: info.label,
                    borderColor: info.borderColor,
                    backgroundColor: "rgb(62,149,205, 0.1)",
                    borderWidth:2,
                 
                },
                ]
            },
          
            options: {
                responsive: true, // Instruct chart js to respond nicely.
                maintainAspectRatio: false, // Add
                scale: {
                    min: 0,
                    max: 8,
                    stepSize: 2,
                },
                legend: {
                    position: 'left'
                },
                scales: {
                    r: {
                        grid: {
                            color: 'lightgray',
                            circular: true
                        },
                        angleLines: {
                            color:'lightgray'
                        },
                        
                    } 
                },
                
            }
        });
        return myChart;
	}


	onMount(() => {
        console.log("the component has mounted");
		myChart = drawChart(chartInfo);

    // http://192.168.1.13:8001/?A1=6&A2=5&A3=1&A4=4&B1=4&B2=3&B3=6&B4=3&C1=2&C2=1&C3=7&C4=9&D1=0&D2=2&D3=6&D4=3&
    // http://localhost:5000/?A1=6&A2=5&A3=1&A4=4&B1=4&B2=3&B3=6&B4=3&C1=2&C2=1&C3=7&C4=8&D1=0&D2=2&D3=6&D4=3&

    }); 

    function printChart(chartObject) {
        let a = document.createElement('a');
        a.href = document.getElementById('myChart').toDataURL('image/png', 1.0);
        a.download = 'chart.jpg';
        a.click();
    }

</script>

<main>
	<h3 class="mb-5 text-center">Your data results</h3>
	
    <div class="container">
        <div class="row">
            <div id="myDiv" class="col-7 border border-dark my-1 p-3">
                <canvas id="myChart" style="width:100%;width:{maxWidth + 'px'};height:{maxWidth + 'px'};"></canvas>
            </div>

            <div class="col-4">
                <div class="border border-dark m-1 p-3">
                    <button id="download"
                        class="btn btn-primary mb-3 "
                        on:click="{printChart(myChart)}"
                        >
                        Print graph
                </button>
                    <div class="accordion" id="accordionPanelsStayOpenExample">
                        <div class="accordion-item">
                            <h2 class="accordion-header" id="panelsStayOpen-headingOne"> 
                                <button class="accordion-button {editDetails.info ? '' : 'collapsed'}" on:click={() => editDetails.info = !editDetails.info} type="button" data-bs-toggle="collapse" data-bs-target="#panelsStayOpen-collapseOne" aria-expanded="true" aria-controls="panelsStayOpen-collapseOne">
                                    Chart info
                                </button>
                            </h2>
                            <div id="panelsStayOpen-collapseOne" class="accordion-collapse collapse {editDetails.info ? 'show': ''}" aria-labelledby="panelsStayOpen-headingOne">
                                <div class="accordion-body">
                                    <div class="input-group mb-3">
                                        <span class="input-group-text" id="basic-addon1">Title</span>
                                        <input type="text" class="form-control" placeholder="Title" aria-label="label" aria-describedby="label" bind:value={chartInfo.label} on:input="{updateChartTitle(myChart, chartInfo.label)}">
                                    </div>
                                    <label for="max-width">Current size: {maxWidth}px</label>
                                    <p>
                                        <input type="range" name="max-width" id="max-width" min="350" max="800" step="50" bind:value={maxWidth}>
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="accordion-item height-500">
                            <h2 class="accordion-header" id="panelsStayOpen-headingTwo">
                                <button class="accordion-button {editDetails.data ? '' : 'collapsed'}" on:click={() => editDetails.data = !editDetails.data} type="button" data-bs-toggle="collapse" data-bs-target="#panelsStayOpen-collapseTwo" aria-expanded="false" aria-controls="panelsStayOpen-collapseTwo">
                                    Chart data
                                </button>
                            </h2>
                            <div id="panelsStayOpen-collapseTwo" class="accordion-collapse collapse {editDetails.data ? 'show' : ''}" aria-labelledby="panelsStayOpen-headingTwo"> 
                                <div class="accordion-body">
                                    <ul class="list-group">
                                        {#each graphData as record}
                                            <div class="input-group mb-3">
                                                <input type="text" bind:value={record.key} on:input={updateChartValues(myChart, graphData)} class="form-control" placeholder="empty" aria-label="key">
                                                <span class="input-group-text">:</span>
                                                <input type="number" bind:value={record.value} on:input={updateChartValues(myChart, graphData)} class="form-control" placeholder="Server" aria-label="value">
                                            </div>
                                        {/each}
                                    </ul>
                                        <div class="input-group mb-3">
                                            <label for="" class="w-100">Add new records</label> 
                                            <input type="text" bind:value={newValue.key} class="form-control" placeholder="key" aria-label="key">
                                            <span class="input-group-text">:</span>
                                            <input type="number" bind:value={newValue.value} on:input={updateChartValues(myChart, graphData)} class="form-control" placeholder="value" aria-label="value">
                                            <button on:click={addRecord(newValue, graphData, myChart)}>Add</button>
                                        </div>

                                </div>
                            </div>
                        </div>
                       
                    </div>
                    
                </div>
                                
            </div>
        
        </div>
        
    </div>
	
</main>

<style>
	main {
		
		padding: 1em;
		margin: 0 auto;
	}

	h3 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}

    #myDiv {
        display: grid;
    }

    #myChart {
        justify-self: center;
    }

    .height-500 {
        max-height: 400px;
        overflow-y: auto;
    }
  
	
</style>