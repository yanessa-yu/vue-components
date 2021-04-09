<template>
    <div  :class="{'mobile': IS_MOBILE}">
        <input type="date" :class="clear?'full':''" v-model="date" placeholder="请选择"  @change="inputdate"/>
    </div>
</template>

<script>
export default {
    data(){
        return({
             IS_MOBILE:IS_MOBILE,
             date: null,
             clear:false
        })
    },
    methods: {
        getCurrentDate() {
            var d = new Date(),
                month = '' + (d.getMonth() + 1),
                day = '' + d.getDate(),
                year = d.getFullYear();

            if (month.length < 2) month = '0' + month;
            if (day.length < 2) day = '0' + day;

            return [year, month, day].join('-');
        },
         inputdate(){
            let _this = this
            if(_this.date.length>0){
               _this.clear = true
            }else{
               _this.clear = false
            }
            _this.$emit('getDate',_this.date)
        },
    }
}
</script>

<style>
.mobile input[type=date] {  
    background-color:transparent;  
    FILTER: alpha(opacity=0); /*androd*/  
    -moz-appearance:none;  /*下拉框去掉右侧图标*/
    -webkit-appearance:none;  
} 
input[type=date]{
     border: none;
}
.mobile input[type="date"]:before{
    color:#333;
    content:attr(placeholder);
}
.mobile input[type="date"].full:before {
    color:black;
    content:""!important;
}


</style>


