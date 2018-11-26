<template>
    <transition name="fadeDown">
        <li>
            <input type="checkbox" :id="taskId" />
            <label :for="taskId">{{task}}</label>
            <button @click="removeTask" class="delete" :id="`del-${taskId}`"></button>
        </li>
    </transition>
</template>


<script>
export default {
    name: 'Task',
    data(){
        return {
            checked: false,
            toDoList: this.getTaskList
        }
    },
    props: {
        todos: Array,
        task: String,
        taskId: Number
    },
    calculated: {
        getTaskList(){
            return this.props.todos;
        }
    },
    methods: {
        removeTask: function(e){
            this.$emit('removeTask', e);
        },
    },
    components: {
    }
}
</script>


<style scoped>
    div {
        display: flex;
    }
    div, label {
        flex: 1 1 0;
    }
    label:before {
        display: inline-block;
    }
    li {
        font-family: 'Montserrat';
        font-size: .9em;
        box-sizing: border-box;
        width: 100%;
        transition: all .15s ease-out;
        position: relative;
        overflow: hidden;
    }
    li:hover {
        transform: translateX(-.5rem);
        z-index: 10;
        background: #fff8c9!important;
        width: calc(100% + .5rem);
    }
    button.delete {
        transition: .3s cubic-bezier(0.50, -0.50, 0.2, 1.5);
        box-sizing: border-box;
        border: 3px solid white;
        background: none;
        border-radius: 100%;
        margin-left: .5rem;
        position: absolute;
        right: 1rem;
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
    button.delete:focus {
        border: 4px double yellow;
        outline: none;
    }
    button.delete:before, button.delete:after {
        content: '';
        width: 1rem;
        height: 2px;
        background-color: white;
        transform: rotate(45deg);
        position: absolute;
    }
    button.delete:after {
        transform: rotate(-45deg);
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
</style>