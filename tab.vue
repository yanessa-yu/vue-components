<template>
    <div class="tab flex">
         <router-link :to="'/list/' + (index + 1)"  class="flex-1"
         :class="(curTab == (index + 1))?'act':''" 
         v-for="(item, index) in list"
          @click.native="clickTab(index)">{{item.name}}</router-link>
    </div>
</template>

<script>
export default {
    data:function(){
        return({
            'curTab': this.$route.params.tabCur
        })
    },
    props:['list'],
    methods:{
        clickTab: function(index){
            let _this = this
            _this.curTab = index + 1
            _this.$emit('getTabCur',_this.curTab)
        }
    },
}
</script>

<style scoped>
    
    .tab{
        font-size: .32rem;
        color: #333;
        text-align: center;
        height: .95rem;
        font-weight: 900;
        }
    .flex{
        display: flex;
        display: -webkit-flex;
        }
    .tab a{
        position: relative;
        border-bottom: .01rem solid  #ddd;
        line-height: .95rem;
        height: .95rem;
        color: #333;
        width: 0;
        cursor: pointer;
        box-sizing: border-box;
        }
   
   .tab a.act {
        color: #5e97f6;
    }
    .tab a.act:after {
        content: "";
        position: absolute;
        width: 1.54rem;
        height: .03rem;
        background: #5e97f6;
        bottom: 0;
        left: 50%;
        -webkit-transform: translate(-50%);
        transform: translate(-50%);
}
</style>
