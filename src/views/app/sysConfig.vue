<template>

</template>

<script setup name="sysConfig">
import {getAuthRole, updateAuthRole} from "@/api/system/user";

const route = useRoute();
const {proxy} = getCurrentInstance();
console.log(route,111)
console.log(proxy,111)
</script>

<style scoped>

</style>
