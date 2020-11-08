<template>
<div>
    <vs-row justify="center">
        <vs-col w="1">
            <span>Questions</span>
        </vs-col>
        <vs-col w="1">
            <span>Counter: {{queNumber}} /10</span>
        </vs-col>
    </vs-row>
</div>
</template>

<script>

export default {
    name: 'Header',
    props:['queNumber']

}
</script>

<style lang="scss" scoped>

</style>
