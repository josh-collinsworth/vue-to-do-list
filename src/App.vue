<template>
  <div id="app" @keyup.esc="changeAlerting">
    <transition name="fadeDown" >
      <Modal 
        v-if="modal.alerting" 
        :type="modal.type"
        :alerting="modal.alerting"
        :modalMessage="modal.message"
        @changeAlerting="changeAlerting"
        @modalConfirmed="modal.type"
      />
    </transition>
    <h1>
      <input 
        :value='listTitle'
        @click='selectListTitle'
        @input='updateListTitle' 
        type="text" 
        id="list-title" 
        placeholder='Qvick List 📝' 
        />
      </h1>
    <Explanation />
    <NewTaskEntry :todos="tasks"/>
    <TaskList 
      :todos="tasks" 
      :taskIsBeingEdited="taskIsBeingEdited" 
      @editTask="editTask" 
      @cancelEdit="cancelEdit" 
      @removeTask="deleteTask" 
      @changeChecked="changeChecked" 
      @updateTaskOrder="updateTaskOrder" 
    />
    <ButtonBar
      :allAreChecked="getAllChecked"
      :anyAreChecked="getAnyChecked"
      :areThereTasks="areThereTasks"
      @selectAll="selectAll"
      @deleteAllChecked="deleteAllChecked"
      @deleteAll="deleteAll"
    />
  </div>
</template>


<script>
  import NewTaskEntry from '@/components/NewTaskEntry';
  import TaskList from '@/components/TaskList';
  import Modal from '@/components/Modal';
  import ButtonBar from '@/components/ButtonBar';
  import Explanation from '@/components/Explanation';

  export default {
    name: 'App',
    data() {
      return {
        listTitle: '',
        tasks: [],
        modal: {
          type: this.changeAlerting,
          message: '',
          alerting: false,
        },
        allAreChecked: this.getAllChecked,
        anyAreChecked: this.getAnyChecked,
        taskIsBeingEdited: false,
        taskBeingEdited: null,
        editedTaskName: ''
      }
    },
    created() {
      const list = JSON.parse(localStorage.getItem('vueToDoList'));
      const title = JSON.parse(localStorage.getItem('vueToDoListTitle'));
      if(list){
        this.tasks = list;
      } else {
        this.tasks = [
          { name: 'Create a new task ☝️', id: 1542895510284, taskChecked: false, editingThisTask: false },
          { name: 'Edit a task 👉', id: 1542895508329, taskChecked: false, editingThisTask: false },
          { name: 'Check a task 👈', id: 1542895508328, taskChecked: false, editingThisTask: false },
          { name: 'Drag tasks to reorder ↕️', id: 1542895508332, taskChecked: false, editingThisTask: false },
          { name: 'Delete a checked task 👉', id: 1542895508330, taskChecked: true, editingThisTask: false },
          { name: 'Try leaving the page or refreshing. 🔄', id: 1542895497118, taskChecked: false, editingThisTask: false }
        ]
      }
      //Make sure the status of editing is not stored in local storage
      this.tasks.forEach(task => task.editingThisTask = false);
      if(title){
        this.listTitle = title;
      } else {
        this.listTitle = '';
      }
    },
    methods: {
      updateTaskOrder() {
        const DOMTasks = document.querySelectorAll('#taskList li input')
        let newTasks = [];
        DOMTasks.forEach(DOMtask => {
          const ID = Number(DOMtask.id);
          this.tasks.forEach(task => {
          // eslint-disable-next-line
            console.log(ID, task.id);
            if(task.id === ID){
              newTasks.push(task);
            }
          })
        })

        this.tasks = newTasks;

        this.updateLocalStorage();
      },
      updateLocalStorage() {
        localStorage.setItem('vueToDoList', JSON.stringify(this.tasks));
      },
      selectListTitle() {
        document.querySelector('#list-title').select();
      },
      updateListTitle(e) {
        this.listTitle = e.target.value;
        localStorage.setItem('vueToDoListTitle', JSON.stringify(this.listTitle));
      },
      selectAll() {
        const inputs = document.querySelectorAll('#app input[type="checkbox"]');
        let allChecked = true;
        inputs.forEach(input => {
          !input.checked ? allChecked = false : '';
        });
        if(!allChecked){
          inputs.forEach(input => input.checked = 'checked');
          this.tasks.forEach(task => task.taskChecked = true);
        } else {
          inputs.forEach(input => {
            input.checked = false;
            this.tasks.forEach(task => task.taskChecked = false);
          });
        }
        this.updateLocalStorage();
      },
      deleteTask(e) {
        const id = parseInt(e.target.id.replace('del-', ''));
        this.tasks = this.tasks.filter(task => {
            return task.id != id;
        });
        this.updateLocalStorage();
      },
      deleteAll() {
        this.modal.message = '⚠️ Delete all tasks and reset the list? (WARNING: this cannot be undone!)';
        this.modal.alerting = true; 
        this.modal.type = this.deleteAllConfirmed;
      },
      deleteAllConfirmed() {
        this.tasks = [];
        localStorage.removeItem('vueToDoList');
        localStorage.removeItem('vueToDoListTitle');
        location.reload();
      },
      deleteAllChecked() {
        this.modal.message = '⚠️ Delete all checked tasks? (This action cannot be undone!)';
        this.modal.alerting = true;
        this.modal.type = this.deleteAllCheckedConfirmed;
      },
      deleteAllCheckedConfirmed() {
        const allTasks = Array.from(document.querySelectorAll('input:checked ~ .delete'));
        const deleteIDs = allTasks.map(ID => {
          return parseInt(ID.id.replace('del-', ''));
        });
        const newTasks = this.tasks.filter(task => {
          return deleteIDs.indexOf(task.id) >= 0 ? null : task;
        });

        this.tasks = newTasks;
        this.updateLocalStorage();
      },
      changeAlerting(e) {
        if(e){
          if((e.key == "Escape" && this.modal.alerting) || e.key != "Escape"){
            this.modal.alerting = !this.modal.alerting;
          }
        } else {
            this.modal.alerting = !this.modal.alerting;
        }
      },
      changeChecked(e) {
        const checkedTask = this.tasks.filter(task => task.id == e.target.id)[0];
        if(checkedTask){
          this.tasks.forEach(task => {
            if (task.id == checkedTask.id){
              task.taskChecked ? task.taskChecked = false : task.taskChecked = true; 
            }
          });
          this.updateLocalStorage();
        }
      },
      editTask(obj) {
        if(!this.taskIsBeingEdited){
          this.taskIsBeingEdited = true;
          this.tasks.forEach((task) => {
            task.id == obj.id ? task.editingThisTask = true : task.editingThisTask = false;
          });
          setTimeout(() => {
            document.querySelector('input.editing').select();
          }, 20);
        } else {
          let oldTasks = this.tasks;
          oldTasks.forEach((task, index) => {
            task.id == obj.id && obj.name ? this.tasks[index].name = obj.name : null;
            task.editingThisTask = false;
          });
          this.tasks = oldTasks;
          this.taskIsBeingEdited = false;
        }
        this.updateLocalStorage();
      },
      cancelEdit() {
        this.taskIsBeingEdited = false;
        this.tasks.forEach(task => task.editingThisTask = false);
      }
    },
    computed: {
      getAllChecked() {
        const allCheckedNow = this.tasks.filter(task => task.taskChecked == true);
        return allCheckedNow.length == this.tasks.length;
      },
      getAnyChecked() {
        const anyCheckedNow = this.tasks.filter(task => task.taskChecked == true);
        return anyCheckedNow.length > 0;
      },
      areThereTasks() {
        return this.tasks.length > 0 ? true : false;
      }
    },
    components: {
      NewTaskEntry, 
      TaskList,
      Modal,
      ButtonBar,
      Explanation
    }
  }
</script>


<style>
  :root {
    --jcBlack: #53565a;
    --jcYellow: #ffd100;
    --lightGray: #a7a8aa;
    --transition-time: .3s;
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
  h1 {
    margin-bottom: 0;
  }
  h1 input {
    font-size: calc(1.3rem + 2vw);
    font-weight: bold;
    width: 100%;
    border: none;
    margin-bottom: -3px;
    cursor: pointer;
  }
  h1 input:focus {
    border-bottom: 3px double;
    outline: none;
  }
  h1 input:placeholder-shown:after {
    content: '📝';
    position: absolute;
  }
  @media(max-width: 624px){
    h1 input {
      font-size: 2.4rem;
    }
    #app {
      max-width: calc(100% - 24px);
      font-size: 1.3rem;
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
    background-color: #fff;
    transition: all .15s;
  }
  button[disabled]{
    pointer-events: none;
    background-color: var(--lightGray);
    border-color: var(--lightGray);
    opacity: .5;
  }
  button:not([disabled]):not(.delete):not(.edit):not(.cancel):hover {
    background-color: var(--jcYellow);
  }
  button.edit:hover, button.cancel:hover, button.delete:hover {
    transform: scale(1.2);
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
