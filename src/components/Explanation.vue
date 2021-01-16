<template>
	<!-- TODO: this component is not currently used anywhere. Use it or lose it. -->
	<div>
		<button id="explanation-link" @click="toggleExplanation">What is this?</button>
		
		<div :style="`
			height: ${explained ? '10rem' : '0'};
			overflow: hidden;
			transition: all .3s ease-in-out;`
		">
			<transition name="fadeDown" >
				<p id="explanation" v-if="explained" >Qvick List is a fast, easy personal to-do list built with Vue, for when you just need a quick checklist without the trouble. It tracks your list in the browser's local storage, so as long as you keep using the same browser/device, your list will stay saved even if you leave the page and come back.</p>
			</transition>   
		</div>
	</div>
</template>


<script>
export default {
	data: () => ({
		explained: false
	}),

	methods: {
		explain(e) {
			e.preventDefault();
			this.explained = !this.explained;
		}
	}
}
</script>


<style scoped>
p, div {
	font-size: 1rem;
	line-height: 1.4;
}

p a {
	font-size: .8rem;
	font-weight: bold;
}

p a:visited {
	color: #888;
}

p:first-of-type {
	margin-top: .5em;
}

p a#explanation-link {
	color: #34657F;
}
</style>