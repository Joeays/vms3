<template>
    <sys-config/>
    <author-manage/>
</template>

<script>
import SysConfig from "@/views/system/sysConfig.vue";
import AuthorManage from "@/views/system/authorManage.vue";

export default {
    name: 'App',
    components: {
        AuthorManage,
        SysConfig,
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
