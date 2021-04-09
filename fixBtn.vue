<template>
        <router-link :to="href">
         <div class="fix-btn" >{{ text }}</div>
       </router-link>
    
</template>

<script>
export default {
    props:['text','href']
}
</script>

<style scoped>
.fix-btn{
        position: fixed;
        z-index: 100;
        border: 1px solid #ddd;
        background: #5e97f6;
        color: #fff;
        padding: .3rem .2rem;
        right: .1rem;
        bottom: .5rem;
        border-radius: .5rem;
        font-weight: 600;
        font-size: .25rem;
    }
</style>

