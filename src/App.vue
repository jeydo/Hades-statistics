<template>
    <div class="row">
        <div class="col mt-3">
            <Form @addRun="addRun" :runs="runs" />
        </div>
        <div class="col mt-3 list-container overflow-auto">
            <ul class="list-group">
                <Run v-for="(run, index) in runs" :index="index" :run="run" @remove="remove(index)" />
            </ul>
        </div>
    </div>
    <div class="row mt-5">
        <div class="col">
            <Chart :runs="runs" ref="chart-component" />
        </div>
    </div>
</template>

<script>
import Form from './components/Form.vue'
import Run from './components/Run.vue'
import Chart from './components/Chart.vue'

export default {
    name: 'App',
    components: {
       Form, Run, Chart
    },
    data() {
        return {
            runs : JSON.parse(localStorage.getItem('runs') || JSON.stringify([]))
        }
    },
    methods : {
        remove(index) {
            //console.log(index)
            this.runs.splice(index, 1)
            this.saveRuns()
        },
        addRun(run) {
            this.runs.push(run)
            this.saveRuns()
        },
        saveRuns() {
            localStorage.setItem('runs', JSON.stringify(this.runs))
            this.$refs['chart-component'].update()
        },
        scrollDown() {

        }
    }
}
</script>
