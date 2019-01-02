<template>
    <div>
        <p><a id="explanation-link" href="#" @click="explain">What is this?</a></p>
        <transition name="fadeDown" >
            <p id="explanation" v-if="explained">This is a personal project I took on to learn Vue. It's your standard to-do list, built entirely on Vue. It uses local storage to keep track of your list, so as long as you keep using the same browser, your list—including which items are and are not checked—will remain intact even if you leave the page and come back.</p>
        </transition>   
    </div>
</template>


<script>
export default {
    name: 'Explanation',
    data(){
        return {
            explained: false
        }
    },
    props: {
    },
    methods: {
        explain: function(e){
            e.preventDefault();
            this.explained = !this.explained;
        }
    },
    components: {
    }
}
</script>


<style scoped>
    p {
        font-size: .55em;
        line-height: 1.4;
    }
    p a {
        font-weight: bold;
    }
    p a:visited {
        color: #888;
    }
    p:first-of-type {
        margin-top: .5em;
    }
</style>