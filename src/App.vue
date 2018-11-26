<template>
  <div id="app" @keyup.esc="changeAlerting">
    <transition name="fadeDown" >
      <Modal v-if="modal.alerting" :type="this.modal.type" :alerting="this.modal.alerting" :modalMessage="this.modal.message" @changeAlerting="changeAlerting" @modalConfirmed="this.modal.type"/>
    </transition>
    <h1>Vue To-Do List</h1>
    <NewTaskEntry :todos="this.tasks"/>
    <TaskList :todos="this.tasks" @removeTask="this.deleteTask"/>
    <ButtonBar @selectAll="selectAll" @deleteAllChecked="deleteAllChecked" @deleteAll="deleteAll" />
  </div>
</template>


<script>
import NewTaskEntry from '@/components/NewTaskEntry';
import TaskList from '@/components/TaskList';
import Modal from '@/components/Modal';
import ButtonBar from '@/components/ButtonBar'

export default {
  name: 'App',
  data() {
    return {
      tasks: [],
      modal: {
        type: this.changeAlerting,
        message: '',
        alerting: false,
      }
    }
  },
  created: function(){
    const list = JSON.parse(localStorage.getItem('vueToDoList'));
    if(list){
      this.tasks = list;
    } else {
      this.tasks = [
        { name: 'Create a new task', id: 1542895510284 },
        { name: 'Delete a task', id: 1542895508328 },
        { name: 'Try leaving the page or refreshing. (The app will remember your tasks using local storage.)', id: 1542895497118 }
      ]
    }
  },
  methods: {
    selectAll: function(){
      const inputs = document.querySelectorAll('#app input[type="checkbox"]');
      let allChecked = true;
      inputs.forEach(input => {
        !input.checked ? allChecked = false : '';
      });
      if(!allChecked){
        inputs.forEach(input => input.checked = 'checked');
      } else {
        inputs.forEach(input => {
          input.checked = false;
        });
      }
    },
    deleteTask: function(e){
      const id = parseInt(e.target.id.replace('del-', ''));
      this.tasks = this.tasks.filter(task => {
          return task.id != id;
      });
      localStorage.setItem('vueToDoList', JSON.stringify(this.tasks));
    },
    deleteAll: function(){
      this.modal.message = '⚠️ Delete all tasks? (WARNING: this cannot be undone!)';
      this.modal.alerting = true; 
      this.modal.type = this.deleteAllConfirmed;
    },
    deleteAllConfirmed: function(){
      this.tasks = [];
      localStorage.removeItem('vueToDoList');
      location.reload();
    },
    deleteAllChecked: function(){
      this.modal.message = '⚠️ Delete all checked tasks? (This action cannot be undone!)';
      this.modal.alerting = true;
      this.modal.type = this.deleteAllCheckedConfirmed;
    },
    deleteAllCheckedConfirmed: function(){
      const allTasks = Array.from(document.querySelectorAll('input:checked ~ .delete'));
      const deleteIDs = allTasks.map(ID => {
        return parseInt(ID.id.replace('del-', ''));
      });
      const newTasks = this.tasks.filter(task => {
        return deleteIDs.indexOf(task.id) >= 0 ? null : task;
      });

      this.tasks = newTasks;
      localStorage.setItem('vueToDoList', JSON.stringify(this.tasks));
    },
    changeAlerting: function(){
      this.modal.alerting = !this.modal.alerting;
    }
  },
  components: {
    NewTaskEntry, 
    TaskList,
    Modal,
    ButtonBar
  }
}
</script>


<style>
  :root {
    --jcBlack: #53565a;
  }
  #app {
    font-family: 'Montserrat', Helvetica, Arial, sans-serif;
    font-size: calc(.7em + 1vw);
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: var(--jcBlack);
    width: 100%;
    max-width: 600px;
    margin: auto;
    padding: 2rem 0 4rem;
  }
  @media(max-width: 624px){
    #app {
      max-width: calc(100% - 24px);
    }
  }
  button {
    font-family: 'Montserrat';
    background: transparent;
    border: 1px solid var(--jcBlack);
    color: var(--jcBlack);
    padding: .5rem 1rem;
    margin: 0;
    font-size: 1rem;
    font-weight: bold;
    cursor: pointer;
    transition: color .15s, background-color .15s;
  }
  button:hover {
    color: white;
    background-color: var(--jcBlack);
  }
  button:first-of-type{
    margin-left: 0;
  }
  button:focus {
    outline: 1px solid var(--jcBlack);
    outline-offset: 1px;
  }
  .fade-enter-active, .fade-leave-active {
    transition: all .2s;
  }
  .fade-enter, .fade-leave-to {
      opacity: 0;
  }
  .fadeDown-enter-active, .fadeDown-leave-active {
      transition: all .25s ease;
  }
  .fadeDown-enter, .fadeDown-leave-to {
      opacity: 0;
      transform: translateY(-1.2em);
  }
  input {
    color: var(--jcBlack);
  }
</style>
