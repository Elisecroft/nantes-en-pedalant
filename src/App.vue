<template>
    <div id="app">
        <HereMap
            appId={ config.appId }
            appCode={ config.appCode }
            lat="47.2205"
            lng="-1.5600"
            width="100%"
            height="835px" />
    </div>
</template>

<script>
    const config = require('./components/secret')

    import HereMap from "./components/HereMap.vue"
    console.log(config)

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