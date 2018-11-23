<template>
    <div id="newTaskBar">
        <input type="text" v-model="newTask" @keyup.enter="addTask">
        <button @click="addTask">Add task</button>
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
                this.todos.unshift({name: this.newTask, id: Date.now()});
                localStorage.setItem('vueToDoList', JSON.stringify(this.todos));
                this.newTask = '';
                this.message = ' ';
                //TO DO: add messaging for blank entry
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
        font-family: 'Avenir';
    }
    #newTaskBar {
        margin-top: 3rem;
        width: 100%;
        display: grid;
        grid-gap: 1rem;
        grid-template-columns: 1fr;
    }
    @media(min-width: 600px){
        #newTaskBar {
            grid-template-columns: 1fr 102px;
        }
    }
    input[type="text"]{
        border: none;
        border-bottom: 1px solid;
        font-size: 1.5rem;
        margin: 0 0 2px 0;
        position: relative;
        top: 3px;
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
</style>