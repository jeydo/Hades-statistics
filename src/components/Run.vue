<template>
    <li class="list-group-item">
        <div class="d-flex w-100 justify-content-between">
            <div class="mb-1">
                <span class="badge bg-primary">{{ run.number }}</span> {{ run.location }}
            </div>
            <small class="text-muted"><a href="#" @click.prevent="remove(index)" class="badge bg-secondary">X</a></small>
        </div>
        <p class="mb-1">
            <span class="badge bg-info">{{ run.weapon }}</span> <span class="badge bg-info">{{ run.aspect }}</span> <span class="badge bg-danger">{{ run.heat }}</span>
        </p>
    </li>
</template>

<script>
    export default {
        name: 'Run',
        props: ['run', 'index'],
        data() {
            return {

            }
        },
        methods : {
            remove(index) {
                this.$emit('remove', index)
            }
        }
    }
</script>
