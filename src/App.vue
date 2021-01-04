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
    <div class="row mt-3 mb-3">
        <div class="col">
            <button type="button" class="btn btn-primary" @click="downloadChart()">Download Chart</button> <button type="button" class="btn btn-info" @click="displayExportData = !displayExportData">Export Data</button> <button type="button" class="btn btn-info" @click="toggleImport">Import Data</button>
        </div>
    </div>
    <div class="row mb-3" v-if="displayExportData">
        <div class="col user-select-all border">
            {{ runs }}
        </div>
    </div>
    <div class="row mb-3" v-if="displayImportData">
        <form>
            <p v-if="importErrors" class="bg-danger">{{ importErrors }}</p>
            <textarea class="form-control" v-model="dataToImport"></textarea>
            <button type="button" class="btn btn-info" @click="importData">Import</button>
        </form> 
    </div>
</template>

<script>
import Form from './components/Form.vue'
import Run from './components/Run.vue'
import Chart from './components/Chart.vue'
import consts from './const'

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
            runs : JSON.parse(localStorage.getItem('runs') || JSON.stringify([])),
            displayExportData : false,
            displayImportData : false,
            dataToImport : '',
            importErrors : ''
        }
    },
    methods : {
        remove(index) {
            this.runs.splice(index, 1)
            this.saveRuns()
        },
        redrawChart() {
            this.$refs['chart-component'].update()
        },
        updateVisibility() {
            this.$nextTick(function () {
                this.redrawChart()
                this.scrollDownList()
            })
        },
        addRun(run) {
            this.runs[run.number - 1] = run
            this.saveRuns()
            this.updateVisibility()
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
        },
        downloadChart() {
            this.$refs['chart-component'].download()
        },
        toggleImport() {
            this.displayImportData = !this.displayImportData
        },
        importData() {
            this.importErrors = ''
            try {
                let runsToImport = JSON.parse(this.dataToImport),
                    newRuns = []
                for (let run of runsToImport) {
                    if (!this.checkRun(run)) {
                        this.importErrors = 'Run is corrupted : ' + JSON.stringify(run)
                        return
                    }
                    newRuns[run.number - 1] = run
                }
                this.runs = newRuns
                this.importErrors = ''
                this.updateVisibility()
                this.toggleImport()
            } catch (e) {
                this.importErrors = 'Impossible to parse datas'
                return
            }
        },
        checkRun(run) {
            if (!run.hasOwnProperty('number') || run.number < 0) {
                return false
            }
            if (!run.hasOwnProperty('location') || !consts.LOCATIONS.includes(run.location)) {
                return false
            }
            if (!run.hasOwnProperty('weapon') || !consts.WEAPONS.hasOwnProperty(run.weapon)) {
                return false
            }
            if (!run.hasOwnProperty('aspect') || consts.WEAPONS[run.weapon].indexOf(run.aspect) < 0) {
                return false
            }
            if (!run.hasOwnProperty('heat') || run.number < 0) {
                return false
            }
            return true
        }
    }
}
</script>
