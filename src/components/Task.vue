<template>
    <li :disabled="taskIsBeingEdited" :class="{editTarget: editingThisTask}">
        <input type="checkbox" @change="changeChecked" :id="taskId" :checked="taskChecked"/>
        <label v-if="!editingThisTask" :for="taskId" >{{task}}</label>
        <input v-else @input="updateNewTaskName" @keydown.enter.prevent="handleEdit" @keydown.escape="cancelEdit" :value="task" autofocus onfocus="this.select()" class="editing" type="text" />
        <div class="button-box"  :class="{'taskIsBeingEdited': taskIsBeingEdited && editingThisTask}">
            <button @click="cancelEdit" :disabled="!taskIsBeingEdited || !editingThisTask" class="cancel" alt="Cancel editing task"><span role="img" aria-label="Cancel editing task" title="Cancel">❌</span></button>
            <button @click.prevent="handleEdit" :disabled="!taskIsBeingEdited || !editingThisTask" class="ok" alt="OK"><span role="img" aria-label="OK" title="OK">🆗</span></button>
        </div>
        <button @click.prevent="handleEdit" :disabled="taskChecked || taskIsBeingEdited" class="edit" :class="{'taskIsBeingEdited': taskIsBeingEdited, 'editingThisTask': editingThisTask}" alt="Edit task"><span role="img" aria-label="Edit task" title="Edit task">📝</span></button>
        <button @click="removeTask" class="delete" :id="`del-${taskId}`" :tabindex="tabableWhenChecked" :aria-hidden="taskChecked ? false : true" alt="Delete task" title="Delete task"></button>
    </li>
</template>


<script>
export default {
    name: 'Task',
    data(){
        return {
            checked: false,
            newTaskName: ''
        }
    },
    props: {
        todos: Array,
        task: String,
        taskId: Number,
        taskChecked: Boolean,
        taskIsBeingEdited: Boolean,
        editingThisTask: Boolean
    },
    computed: {
        tabableWhenChecked: function(){
            return this.taskChecked ? "0" : "-1";
        },
        tabableWhenUnchecked: function(){
            return this.taskChecked ? "-1" : "0";
        }
    },
    methods: {
        removeTask: function(e){
            this.$emit('removeTask', e);
        },
        changeChecked: function(e){
            this.$emit('changeChecked', e);
        },
        handleEdit: function(){
            this.$emit('editTask', {id: this.taskId, name: this.newTaskName});
        },
        cancelEdit: function(){
            this.newTaskName = '';
            this.$emit('cancelEdit');
        },
        updateNewTaskName: function(e){
            this.newTaskName = e.target.value;
        }
    }
}
</script>


<style scoped>
    div {
        display: flex;
    }
    div, label {
        flex: 1 1 0;
        max-width: 92%;
    }
    label:before {
        display: inline-block;
    }
    li {
        font-family: 'Montserrat';
        font-size: .9em;
        box-sizing: border-box;
        width: 100%;
        overflow: hidden;
    }
    li:hover {
        /* transform: scale(1.01); */
        z-index: 10;
        background: #dbf8ff!important;
        /* width: calc(100% + .5rem); */
    }
    li:hover, label:hover {
        cursor: pointer;
    }
    .isBeingEdited {
        opacity: .5;
    }
    .editTarget {
        opacity: 1;
    }
    .delete {
        transition: .3s cubic-bezier(0.50, -0.50, 0.2, 1.5);
        box-sizing: border-box;
        border: 3px solid white;
        background: none;
        border-radius: 100%;
        margin-left: .5rem;
        position: absolute;
        right: 0.5rem;
        top: calc(100%);
        width: 2rem;
        height: 2rem;
        background: red;
        color: white;
        display: flex;
        padding-top: 100%;
        text-align: center;
        justify-content: center;
        align-items: center;
        align-content: center;
        cursor: pointer;
        padding: 0;
    }
    .delete:focus {
        border: 4px double yellow;
        outline: none;
    }
    .delete:before, .delete:after {
        content: '';
        width: 1rem;
        height: 2px;
        background-color: white;
        transform: rotate(45deg);
        position: absolute;
    }
    .delete:after {
        transform: rotate(-45deg);
    }
    .delete:hover {
        background: darkred;
    }
    input[type="checkbox"]{
        position: absolute;
        left: -100vw;
        width: 0;
        height: 0;
    }
    input[type="checkbox"] + label:before {
        content:'⬜';
        margin-right: .5em;
        padding: .1em;
    }
    input[type="checkbox"]:checked + label {
        text-decoration: line-through;
    }
    input[type="checkbox"]:checked + label:before {
        content:'✅';
        text-decoration: none!important;
    }
    input[type="checkbox"]:focus + label:before {
        outline: 3px double;
        text-decoration: none;
    }
    input[type="checkbox"]:checked  ~ button.delete {
        display: flex;
        top: calc(50% - 1rem);
    }
    input.editing {
        font-family: 'Montserrat';
        font-size: 1em;
        margin-left: 1.5em;
        padding: 0.1em;
    }
    .edit, .cancel, .ok {
        position: relative;
        top: 0rem;
        transition: all .3s cubic-bezier(0.50, -0.50, 0.2, 1.5);
        padding: 0;
        border: none;
        font-size: 1em;
        background: inherit;
    }
    .ok, .edit {
        margin-left: .5em;
        top: 0;
    }
    .edit {
        position: absolute;
        right: 0.5em;
        top: calc(50% - .65em);
    }
    input[type="checkbox"]:checked ~ .edit {
        top: calc(-4em);
    }
    .button-box {
        z-index: 12;
        top: calc(-100%);
        right: 0.5em;
        position: absolute;
        width: 2.5em;
        max-width: 2.5em;
        display: flex;
        justify-content: flex-end;
        align-items: center;
        align-content: center;
        transition: all .3s cubic-bezier(0.50, -0.50, 0.2, 1.5);
    }
    .button-box button {
        line-height: 1.8em;
    }
    .edit.taskIsBeingEdited.editingThisTask {
        top: calc(100%);
        right: 0.5em;
    }
    .button-box.taskIsBeingEdited {
        top: calc(0% + .5em);
    }
</style>