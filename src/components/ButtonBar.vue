<template>
    <div id="buttonBar">
        <button @click="$emit('selectAll')">Check all</button>
        <button @click="$emit('deleteAllChecked')">Delete All Checked</button>
        <button @click="$emit('deleteAll')">Delete All and Reset</button>
    </div>
</template>


<script>
export default {
    name: 'ButtonBar',
    props: {
    },
    components: {
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