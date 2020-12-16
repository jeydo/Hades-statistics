<template>
    <div class="form">
        <div class="mb-3">
            <label for="run-number" class="form-label">Run #</label>
            <input type="number" name="run" id="run-number" class="form-control form-control-sm" v-model="run.number" />
        </div>
        <div class="mb-3">
            <label class="form-label">Location</label>
            <div>
                <div class="form-check form-check-inline" v-for="location in consts.LOCATIONS">
                    <input class="form-check-input" name="location" type="radio" :id="'l-' + location" :value="location" v-model="run.location">
                    <label class="form-check-label" :for="'l-' + location">{{ location }}</label>
                </div>
            </div>
        </div> 
        <div class="mb-3">
            <label class="form-label">Weapons</label>
            <div>
                <div class="form-check form-check-inline" v-for="(aspects, weapon) in consts.WEAPONS">
                    <input class="form-check-input" name="weapon" type="radio" :id="weapon" :value="weapon" v-model="run.weapon" @change="run.aspect = ''">
                    <label class="form-check-label" :for="weapon">{{ weapon }}</label>
                </div>
            </div>
        </div> 
        <div class="mb-3">
            <label class="form-label">Aspect</label>
            <div v-if="run.weapon != ''">
                <div class="form-check form-check-inline" v-for="aspect in consts.WEAPONS[run.weapon]">
                    <input class="form-check-input" name="aspect" type="radio" :id="aspect" :value="aspect" v-model="run.aspect">
                    <label class="form-check-label" :for="aspect">{{ aspect }}</label>
                </div>
            </div>
        </div>
        <div class="mb-3">
            <label for="heat" class="form-label">Heat</label>
            <input type="number" name="heat" id="heat" class="form-control form-control-sm" v-model="run.heat" />
        </div>

        <button type="button" class="btn btn-primary" @click="addRun()">Add Run</button>
    </div>
</template>

<script>
import consts from '../const'

export default {
  name: 'Form',
  created () {
    this.consts = consts;
  },
  data() {
    return {
        run : {
            number : 1,
            location : '',
            weapon : '',
            aspect : '',
            heat : 0
        }
    }
  },
  methods : {
    addRun() {
        this.$emit('add-run', Object.assign({}, this.run))
    }
  }
}
</script>
