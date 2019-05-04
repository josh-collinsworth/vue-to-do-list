<template>
    <div id="buttonBar">
        <button @click="$emit('selectAll')" :disabled="areThereTasks ? null : 'disabled'"><span v-if="allAreChecked">Unc</span><span v-else>C</span>heck all</button>
        <button @click="$emit('deleteAllChecked')" :disabled="anyAreChecked ? null : 'disabled'">Delete All Checked</button>
        <button @click="$emit('deleteAll')">Delete All and Reset</button>
    </div>
</template>


<script>
export default {
    name: 'ButtonBar',
    props: {
        allAreChecked: Boolean,
        anyAreChecked: Boolean,
        areThereTasks: Boolean
    }
}
</script>


<style scoped>
    #buttonBar {
        display: grid;
        grid-template-columns: auto auto auto auto;
        grid-gap: 1rem;
        grid-template-columns: auto;
        justify-content: stretch;
    }
    @media(min-width: 600px){
        #buttonBar {
            grid-template-columns: repeat(3, auto);
            justify-content: end;
        }
    }
</style>