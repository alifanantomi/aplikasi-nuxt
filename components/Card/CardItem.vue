<template>
    <div 
        :class="[
            'item-task d-flex align-items-start border-bottom pt-3 pb-4', 
            isGrid ? 'col-6 col-md-4' : 'col-12' 
        ]" 
    >
        <input 
            type="checkbox" 
            name="status" 
            id="task" 
            class="me-2 mt-2" 
            :checked="task.isDone" 
            v-model="task.isDone"
        >
        <div :class="['d-flex flex-column', task.isDone ? 'text-decoration-line-through fst-italic' : '']">
            <div class="title-task mb-1">
                {{ task.title }}
            </div>
            <div class="description-task small text-muted">
                {{ task.description }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        task: {
            type: Object,
            default: 'Untitled'
        },
        isGrid: {
            type: Boolean,
            required: true,
            default: false
        }
    }
}
</script>

<style>

</style>