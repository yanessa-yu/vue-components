<template>
   <div class="v-list">
            <div class="v-item flex" v-for="(item ,index) in list">
                <router-link :to="item.href"  > </router-link>
                        <div class="v-item-l">
                            <div class="v-item-i" :class="(i=='title')?'title':''" 
                            v-if="(i!== 'href' && i != 'buttons' && i != 'id' && i != 'texts')"
                            v-for="(v,i) in item">{{ v }}</div>
                        </div>
               
                <div class="v-item-r flex">
                    <div v-for="(v,i) in item.buttons" >
                       <button  v-if="v.flag" @click="clickButtonSon(item.id,i,v.text)">{{ v.text }}</button>
                    </div>
                    <div v-for="(v,i) in item.texts">
                        <div v-if="v.flag">{{ v.text }}</div>
                    </div>
                </div>
            </div>
   </div>
</template>

<script>
export default {
    props:['list','clickButton'],
    methods:{
        clickButtonSon: function(id,index,text){
            let _this = this
            _this.$emit( 'clickButton', {'id':id,'index':index + 1,'text':text} )
        },
        setHeight:function(){
            $(".v-item").each(function (index) {
                $(".v-item a").height($(this).height())
            });
        }
    },
    mounted(){
        let _this = this
        _this.$nextTick(function(){
            if($(".v-item").length == 0){
                setTimeout(function(){
                    _this.setHeight()
                }, 300) ;
            }else{
                _this.setHeight()
            }
        })
    }
}
</script>

<style scoped>
    .v-list .v-item{
        padding: .3rem;
        margin: 0 .2rem;
        border-bottom: .01rem solid #ddd;
    }
    .flex{
        display: -webkit-flex;
        display: flex;
        justify-content:space-between;
    }
    .flex0{
        flex: 0;
    }
    .flex1{
        flex: 1;
    }
    .title{
        font-weight: 600;
    }
    a{
        color: #333;
    }
    .v-item .v-item-r button{
        background: #5e97f7;
        color: #fff;
       
    }
    .v-item-r div{
        margin-left: .1rem;
        z-index: 10;
     }

    .v-item a{
        position: absolute;
        width: 100%;
        height: 40px;
        z-index:0;
    }
    

</style>


