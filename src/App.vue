<template>
    <div class="row">
        <div class="col">
            <Form @addRun="addRun" />
        </div>
        <div class="col">
            <Run v-for="(run, index) in runs" :key="index" :run="run" @remove="remove(index)" />
        </div>
    </div>
</template>

<script>
import Form from './components/Form.vue'
import Run from './components/Run.vue'

export default {
    name: 'App',
    components: {
       Form, Run
    },
    data() {
        return {
            runs : []
        }
    },
    methods : {
        remove(index) {
            delete(this.runs[index]) 
        },
        addRun(event) {
            this.runs.push(event)
        }
    }
}
</script>
