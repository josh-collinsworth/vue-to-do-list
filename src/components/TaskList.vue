<template>
    <ul>
        <Task v-for="task in todos" :key="task.id" :task-id="task.id" :task="task.name" :todos="todos" @removeTask="removeTask">{{task}}</Task>
    </ul>
</template>


<script>
import Task from '@/components/Task'

export default {
    name: 'TaskList',
    data() {
        return {
        }
    },
    methods: {
        removeTask: function(e){
            this.$emit('removeTask', e);
        }
    },
    props: ['todos'],
    components: {
        Task
    }
}
</script>


<style scoped>
    ul {
        list-style-type: 0;
        padding: 0;
        margin-bottom: 4rem;
    }
    li {
        position: relative;
        padding: .5em;
        display: flex;
        justify-content: space-between;
        align-items: center;
        align-content: center;
    }
    li:nth-of-type(odd){
        background: #f8f8f8;
    }
</style>