<script>
    import { onMount, afterUpdate } from "svelte";
    import Chart from "chart.js/auto";

    export let productivityData = [];

    let chartCanvas;

    let chartInstance = null;

    const renderChart = () => {
        if (chartInstance) {
            chartInstance.destroy();  // Destroy the old chart before creating a new one
        }

        chartInstance = new Chart(chartCanvas, {
            type: 'pie',
            data: {
                labels: ['Skill Practice', 'Yoga Duration', 'Water Intake'],
                datasets: [{
                    label: 'Productivity Breakdown',
                    data: productivityData,
                    backgroundColor: ['#03dac6', '#6110ee', '#b00020'],
                    hoverOffset: 4
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top',
                    },
                    tooltip: {
                        callbacks: {
                            label: function (tooltipItem) {
                                return tooltipItem.label + ': ' + tooltipItem.raw ;
                            }
                        }
                    }
                }
            }
        });
    };

    onMount(() => {
        renderChart();
    });

    afterUpdate(() => {
        renderChart();
    });
</script>

<canvas bind:this={chartCanvas}></canvas>

<style>
    canvas {
        max-width: 100%;
        margin: 0 auto;
    }
</style>
