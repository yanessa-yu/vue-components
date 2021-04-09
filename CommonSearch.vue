<template>
    <div class="search-box flex">
        <input type="text" class="search-input flex-1" :placeholder="placeholder" v-model="searchText" />
        <a href="javascript:void(0);" class="search-btn" @click="searchCommon">搜索</a>
    </div>
</template>

<script>
export default {
    props:['placeholder'],
    data(){
        return({
            searchText:'',
        })
    },
    methods:{
        searchCommon(){
            let _this = this
            _this.$emit('search',_this.searchText)
        }
    }
}
</script>

<style scoped>
    .search-box{
        position: relative;
        line-height: .64rem;
        margin: .3rem .1rem 0 .3rem;
    }
    .search-box:before {
        content: "";
        position: relative;
        display: inline-block;
        width: .3rem;
        height: .3rem;
        left: .6rem;
        top: .06rem;
        background: url(../assets/images/common-icon.png?) no-repeat -.35rem -.05rem/2rem;
    }
    .search-input{
        display: block;
        padding: .14rem .7rem;
        line-height: .36rem;
        background: #f0f0f0;
        border-radius: 1rem;
        border: 0;
        outline: none;
        font-size: .26rem;
        color: #333;
    }
    .search-btn{
        display: block;
        width: 1.06rem;
        text-align: center;
        font-size: .28rem;
        color: #111;
    }
</style>

