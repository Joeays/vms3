<script setup>
// 如果使用defineProps接收数据，这个数据只能在模板中渲染，
// 如果想要在script中也操作props属性，应该接收返回值。

const props = defineProps({
    sonMoney: {
        type: Number,
        default: 1
    }
})

console.log('setup', props.sonMoney)
</script>

<template>
    son
    {{sonMoney}}
</template>
