<template>
    <div id="app">
        <HereMap
            appId="DiGvwlJhXUFwnnkS1DL0"
            appCode="1sKutluykr4HfVFdV-BKJZ77Sz1SSWTNW3-WF6Lir18"
            lat="47.2205"
            lng="-1.5600"
            width="100%"
            height="835px" />
    </div>
</template>

<script>
    import HereMap from "./components/HereMap.vue"

    export default {
        name: 'app',
        components: {
            HereMap
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
    }
</style>