<template>
    <canvas id="myChart" height="400"></canvas>
</template>

<script>
    import Chart from 'chart.js'
    import consts from '../const'
    
    export default {
        name: 'Chart',
        props: ['runs'],
        created () {
            this.consts = consts;
        },
        mounted() {
            let ctx = document.getElementById('myChart').getContext('2d');
            let chart = new Chart(ctx, this.options);
            this.chart = chart;
        },
        data() {
            let options = {
                type: 'line',
                data: {
                    datasets: [{
                        data: this.chartDataArray(),
                        lineTension : 0,
                        fill: false,
                        backgroundColor:'rgba(0, 0, 0, 1)',
                        borderColor:'rgba(0, 0, 0, 1)'
                    },
                    {
                        type : 'horizontalBar',
                        data: this.chartBar(),
                        categoryPercentage : 1,
                        barThickness : 36.3,
                        backgroundColor: [
                            'rgba(0, 0, 0, 0)',
                            '#ffd93b',
                            '#f7c7ba',
                            '#f7b9a6',
                            '#4a7092',
                            '#325e84',
                            '#a06441',
                            '#93502b',
                            '#5e7a51',
                            '#4f6e42',
                            'rgba(0, 0, 0, 0)',
                        ]
                    }]
                },
                options : {
                    legend: {
                        display: false
                    },
                    responsive: true,
                    maintainAspectRatio : false,
                    scales : {
                        xAxes: [{
                            labels: this.chartAxis()
                        }],
                        yAxes: [{
                            type : 'category',
                            labels: this.getLabels()
                        }]
                        
                    },
                    tooltips: {
                        intersect : false
                        /*callbacks: {
                            label: function(tooltipItem, data) {
                                //return 'pouet'
                            }
                        }*/
                    }
                }
            };
            return {
                options : options
            }
        },
        methods : {
            getLabels() {
                return [].concat([''], consts.LOCATIONS, ['']).reverse()
            },
            chartDataArray() {
                let arr = [];
                this.runs.forEach((run) => {
                    arr.push(run.location)
                });
                return arr; 
            },
            chartAxis() {
                let arr = [];
                this.runs.forEach((run) => {
                    arr.push(run.number)
                })
                return arr;
            },
            chartBar() {
                let arr = [], length = this.runs.length;
                this.getLabels().forEach((run) => {
                    arr.push(length)
                })
                return arr;
            },
            update() {
                this.chart.data.datasets[0].data = this.chartDataArray()
                this.chart.data.datasets[1].data = this.chartBar()
                this.chart.options.scales.xAxes[0].labels = this.chartAxis()
                this.chart.update()
            }
        }
    }
</script>