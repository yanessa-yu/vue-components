<template>
    <div  class="v-submit" @click="submitHandlerSon"> {{text }}</div>
</template>

<script>
export default {
    props: ['text'],
    methods:{
        submitHandlerSon(){
            this.$emit('submitHandler')
        }
    }
}
</script>

<style scoped>
.v-submit{
    text-align: center;
    height: 1rem;
    position: fixed;
    bottom: 0;
    background: #5e97f6;
    width: 100%;
    line-height: 1rem;
    color: #fff;
    font-size: .4rem;
}
</style>

