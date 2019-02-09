<template>
    <ul>
        <Task v-for="task in todos" 
            :key="task.id" 
            :task-id="task.id" 
            :task="task.name" 
            :todos="todos" 
            :taskChecked="task.taskChecked"
            :taskIsBeingEdited="taskIsBeingEdited"
            :editingThisTask="task.editingThisTask"
            @removeTask="removeTask"
            @changeChecked="changeChecked"
            @editTask="editTask"
            @cancelEdit="cancelEdit"
        >
            {{task}}
        </Task>
    </ul>
</template>


<script>
import Task from '@/components/Task'

export default {
    name: 'TaskList',
    methods: {
        removeTask: function(e){
            this.$emit('removeTask', e);
        },
        changeChecked: function(e){
            this.$emit('changeChecked', e);
        },
        editTask: function(id){
            this.$emit('editTask', id);
        },
        cancelEdit: function(){
            this.$emit('cancelEdit');
        }
    },
    props: {
        'todos': Array,
        'taskIsBeingEdited': Boolean
    },
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