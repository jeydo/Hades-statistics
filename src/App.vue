<template>
    <div class="row">
        <div class="col mt-3">
            <Form @addRun="addRun" :lastRun="runs[runs.length - 1]" />
        </div>
        <div class="col mt-3 list-container overflow-auto" ref="list-container">
            <ul class="list-group">
                <template v-for="(run, index) in runs">
                    <Run  v-if="run" :index="index" :run="run" @remove="remove(index)" />
                </template> 
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
    mounted() {
        this.scrollDownList()
    },
    data() {
        return {
            runs : JSON.parse(localStorage.getItem('runs') || JSON.stringify([]))
        }
    },
    methods : {
        remove(index) {
            this.runs.splice(index, 1)
            this.saveRuns()
        },
        addRun(run) {
            this.runs[run.number - 1] = run
            /*if (this.runs.length != run.number) {
                this.reOrderRuns()
            }*/
            this.saveRuns()
            this.$refs['chart-component'].update()
            this.$nextTick(function () {
                this.scrollDownList()
            })
        },
        saveRuns() {
            localStorage.setItem('runs', JSON.stringify(this.runs))
        },
        scrollDownList() {
            this.$refs['list-container'].scrollTop = this.$refs['list-container'].scrollHeight
        },
        getCurrentRunNumber() {
            return this.runs.length ? parseInt(this.runs[this.runs.length - 1].number) + 1 : 1
        },
        reOrderRuns() {
            let runs = []
            this.runs.forEach((run) => {
                runs[run.number - 1] = run
            })
            this.runs = runs
        }
    }
}
</script>
