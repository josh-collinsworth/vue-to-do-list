<template>
    <div class="modal-bg">
        <transition name="fadeDown">
            <div class="modal" v-if="alerting">
                <p>{{modalMessage}}</p>
                <button @click="$emit('changeAlerting')">Cancel</button>
                <button @click="this.confirm" ref="confirm">OK</button>
            </div>
        </transition>
    </div>
</template>


<script>
export default {
    name: 'Modal',
    props: {
        modalMessage: String,
        modalConfirmation: Boolean,
        alerting: Boolean,
        type: Function
    },
    mounted: function() {
        this.$refs.confirm.focus();
    },
    methods: {
        changeAlerting: function() {
            console.log(this.$refs);
            this.$emit('changeAlerting');
        },
        confirm: function(){
            this.$emit('changeAlerting');
            this.$emit('modalConfirmed');
        },
    },
    components: {
    }
}
</script>


<style scoped>
    .modal-bg, .modal-bg:before, .modal-bg:after {
        content: '';
        width: 100vw;
        height: 100vh;
        z-index: 100;
        position: fixed;
        top: 0;
        left: 0;
        background: rgba(0,0,0,.2);
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .modal-bg:before {
        top: -200vh;
    }
    .modal-bg:after {
        top: 100vh;
    }
    .modal {
        padding: 2rem;
        background: rgba(255,255,255,.95);
        z-index: 101;
        width: 360px;
        /* display: flex;
        flex-wrap: wrap;
        justify-content: flex-end; */
        display: grid;
        grid-template-columns: repeat(2, auto);
        grid-gap: 1rem;
        place-content: end;
    }
    p {
        width: 100%;
        grid-column: 1 / -1;
    }
</style>