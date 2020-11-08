<template>
    <div class="container">
       <slot></slot>
    </div>
</template>

<script>
    export default {
        name: "Container"
    }
</script>

<style lang="scss" scoped>
 .container{
     width: 50%;
     padding: 25px;
     background-color: blueviolet;
 }

</style>