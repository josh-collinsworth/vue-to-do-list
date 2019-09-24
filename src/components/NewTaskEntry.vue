<template>
    <div id="newTaskBar">
        <input
            id="newTaskBarInput"
            type="text"
            v-model="newTask"
            @keyup.enter="addTask"
            placeholder="Add a new task"
        />
        <button @click="addTask">Add task</button>
        <label for="newTaskBarInput">Add a new task</label>
        <p>{{message}}&nbsp;</p>
    </div>
</template>


<script>
export default {
    name: 'NewTaskEntry',
    data(){
        return {
            newTask: '',
            message: ' '
        }
    },
    methods: {
        addTask: function(){
            if(!this.todos.includes(this.newTask) && this.newTask){
                this.todos.unshift({name: this.newTask, id: Date.now(), taskChecked: false});
                localStorage.setItem('vueToDoList', JSON.stringify(this.todos));
                this.newTask = '';
                this.message = ' ';
            } else if (!this.newTask){
                this.message = '⚠️ Added tasks cannot be blank.'
            } else{
                this.message = '⚠️ This task is already on the list.'
            }
        }
    },
    props: {
        todos: Array
    },
    components: {
    }
}
</script>


<style scoped>
    * {
        font-family: 'Montserrat';
    }
    #newTaskBar {
        margin-top: 4rem;
        width: 100%;
        display: grid;
        grid-gap: 1rem;
        grid-template-columns: 1fr;
    }
    @media(min-width: 600px){
        #newTaskBar {
            grid-template-columns: 1fr 110px;
        }
    }
    input[type="text"]{
        border: none;
        border-bottom: 1px solid;
        font-size: 1.5rem;
        margin: 0 0 2px 0;
        position: relative;
        top: 3px;
        overflow: visible;
        background: #fff;
        z-index: 3;
        grid-row: 1 / 2;
        grid-column: 1 / 2;
    }
    ::placeholder {
        color: #ddd;
    }
    input[type="text"]:focus{
        outline: none;
        border-bottom: 3px double;
        margin-bottom: 0px;
    }
    p {
        color: red;        
        margin-bottom: 3rem;
        font-size: .9rem;
        font-weight: bold;
        position: relative;
    }
    button {
        justify-self: flex-end;
    }
    p, label {
        grid-column: 1 / -1;
    }
    label {
        color: #a7a8aa;
        font-size: .5em;
        font-weight: bold;
        position: relative;
        top: .5rem;
        grid-row: 1 / 2;
        grid-column: 1 / 2;
        transition: top .2s ease-in-out;
    }
    input:not(:placeholder-shown) ~ label {
        top: -1.2rem;
    }
</style>
