<template>
	<!-- TODO: don't think the disabled attribute works on list items 🤦🏻‍♂️ -->
	<li :disabled="taskIsBeingEdited" :class="{editTarget: editingThisTask}" @dragend="updateTaskOrder">
		<input type="checkbox" @change="changeChecked" :id="taskId" :checked="taskChecked"/>
		
		<label v-if="!editingThisTask" :for="taskId" >{{task}}</label>
		<input v-else 
			@input="updateNewTaskName" 
			@keydown.enter.prevent="handleEdit"
			@keydown.escape="cancelEdit"
			:value="task"
			autofocus
			onfocus="this.select()"
			class="editing"
			type="text" 
		/>

		<div class="button-box" :class="{'taskIsBeingEdited': taskIsBeingEdited && editingThisTask}">
			<button 
				@click="cancelEdit"
				:disabled="!taskIsBeingEdited || !editingThisTask"
				class="cancel"
			>
				<span role="img" alt="Cancel editing task" title="Cancel">❌</span>
			</button>

			<button 
				@click.prevent="handleEdit"
				:disabled="!taskIsBeingEdited || !editingThisTask"
				class="ok"
			>
				<span role="img" alt="OK" title="Done">🆗</span>
			</button>
		</div>

		<button
			@click.prevent="handleEdit"
			:disabled="taskChecked || taskIsBeingEdited"
			:class="{'taskIsBeingEdited': taskIsBeingEdited, 'editingThisTask': editingThisTask}"
			class="edit"
		>
			<span role="img" alt="Edit task" title="Edit task">📝</span>
		</button>

		<button
			@click="removeTask"
			:id="`del-${taskId}`"
			:tabindex="tabableWhenChecked"
			:aria-hidden="taskChecked ? false : true"
			aria-label="Delete task"
			title="Delete task"
			class="delete"
		>
		</button>
	</li>
</template>


<script>
export default {
	data: () => ({
		checked: false,
		newTaskName: ''
	}),

	props: {
		todos: Array,
		task: String,
		taskId: Number,
		taskChecked: Boolean,
		taskIsBeingEdited: Boolean,
		editingThisTask: Boolean
	},

	computed: {
		tabableWhenChecked() {
			return this.taskChecked ? "0" : "-1";
		},

		tabableWhenUnchecked() {
			return this.taskChecked ? "-1" : "0";
		}
	},

	methods: {
		removeTask(e) {
			this.$emit('removeTask', e);
		},

		changeChecked(e) {
			this.$emit('changeChecked', e);
		},

		handleEdit() {
			this.$emit('editTask', {id: this.taskId, name: this.newTaskName});
		},

		cancelEdit() {
			this.newTaskName = '';
			this.$emit('cancelEdit');
		},

		updateNewTaskName(e) {
			this.newTaskName = e.target.value;
		},

		updateTaskOrder() {
			// eslint-disable-next-line
			this.$emit('updateTaskOrder')
		}
	}
}
</script>


<style scoped>
div {
	display: flex;
}

div, label {
	flex: 0 1 auto;
	/* max-width: 92%; */
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
	cursor: move;
}

label:hover {
	cursor: pointer;
}

.isBeingEdited {
	opacity: .5;
}

.editTarget {
	opacity: 1;
}

.delete {
	transition: .2s cubic-bezier(0.50, -0.50, 0.2, 1.5);
	box-sizing: border-box;
	border: 3px solid white;
	background: none;
	border-radius: 100%;
	margin-left: .5rem;
	position: absolute;
	right: 0.5rem;
	transform: translateY(calc(175%));
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
	transform: translateY(calc(50% - 1rem));
}

input.editing {
	font-family: 'Montserrat';
	font-size: 0.875em; /* Prevents resizing of list item */
	margin-left: 1.5em;
	padding: 0.1em;
}

.edit, .cancel, .ok {
	position: relative;
	transform: translateY(0rem);
	transition: all .2s cubic-bezier(0.50, -0.50, 0.2, 1.5);
	padding: 0;
	border: none;
	font-size: 1em;
	background: inherit;
}

.ok, .edit {
	margin-left: .5em;
	transform: translateY(0);
}

.edit {
	position: absolute;
	right: 0.5em;
	transform: translateY(calc(50% - .65em));
}

input[type="checkbox"]:checked ~ .edit {
	transform: translateY(calc(-4em));
}

.button-box {
	z-index: 12;
	transform: translateY(calc(-150%));
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
	transform: translateY(calc(150%));
	right: 0.5em;
}

.button-box.taskIsBeingEdited {
	transform: translateY(0);
}

</style>
