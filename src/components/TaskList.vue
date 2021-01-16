<template>
	<ul id="taskList">
		<draggable :todos="todos" >
			<transition-group name="shift" >
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
					@updateTaskOrder="updateTaskOrder"
				>
					{{task}}
				</Task>
			</transition-group>
		</draggable>
	</ul>
</template>


<script>
import Task from '@/components/Task'
import draggable from 'vuedraggable'

export default {
	methods: {
		removeTask(e) {
			this.$emit('removeTask', e);
		},

		changeChecked(e) {
			this.$emit('changeChecked', e);
		},

		editTask(id) {
			this.$emit('editTask', id);
		},

		cancelEdit() {
			this.$emit('cancelEdit');
		},

		updateTaskOrder() {
			this.$emit('updateTaskOrder', this.todos)
		}
	},

	props: {
		'todos': Array,
		'taskIsBeingEdited': Boolean
	},

	components: {
		Task, draggable
	}
}
</script>


<style>
ul {
	list-style-type: 0;
	padding: 0;
	margin-bottom: 4rem;
	position: relative;
}

li {
	position: relative;
	padding: .5em;
	display: flex;
	justify-content: space-between;
	align-items: center;
	align-content: center;
}

li:nth-of-type(odd) {
	background: #f8f8f8;
}

.shift-enter-active, .shift-leave-active {
	transition: all var(--transition-time) ease-in-out!important;
	opacity: 1;
	transform: translateY(0em);
}

.shift-leave-active {
	position: absolute;
}

.shift-enter, .shift-leave-to {
	opacity: 0;
	transform: translateY(-1.5em);
}

.shift-leave-to {
	transform-origin: top;
	z-index: 50;
	transform: translateY(-100%);
}
.shift-move {
	transition: transform var(--transition-time)!important;
}

</style>
