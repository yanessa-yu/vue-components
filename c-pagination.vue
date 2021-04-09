<template>
    <div class="own-pagination">
        <el-pagination layout="total,prev, pager, next" :total="count"  @current-change="handleCurrentChange"
      :current-page="page" :page-size="row"
    ></el-pagination>
    </div>
</template>

<script>

import {Pagination} from "element-ui";

export default {
    props: ['count','page','row'],
    components: {
        'el-pagination':Pagination
    },
    methods: {
        handleCurrentChange(currentPage){
            let _this = this
             this.$emit('changePage',{'page':currentPage,'row':_this.row})
        }
    },
     mounted(){
        let _this = this
        _this.handleCurrentChange(1)
    }
    
}
</script>

<style scoped>
.own-pagination{
    margin: 2px 0;
}
</style>


