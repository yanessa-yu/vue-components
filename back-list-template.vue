<template>
<div>
   <import-excel :importExcelData="importExcelData" :className="propsData.className"  :getList=0  @getImportExcelData="importItemsSave($event)"></import-excel>
    <div class="table">
     <own-pagination :page="page" :count="count" :row="rows" @changePage="changePage" ></own-pagination>
     <div @click="del">11</div>
     <el-table :data="list"  class="table own-main-table"  border size="mini" row-key="id">
            <el-table-column 
                v-for="(row, index) in tableData.cols" 
                :key="row.prop" 
                :label="row.label"  
                :prop="row.prop"  >
            </el-table-column>
            <el-table-column label="操作" v-if="Object.keys(tableData.operCol).length > 0" :width="tableData.operCol.width">
                 <template slot-scope="scope">
                    <el-button 
                    v-for="(r,i) in tableData.operCol.children" 
                    :key="r.prop"
                    size="mini" 
                    :type="r.type" 
                    fixed
                    v-on:click="oper(r.prop, scope.$index, scope.row)" >
                    {{ r.label }}
                    </el-button>
                </template>
            </el-table-column>
     </el-table>
     <own-pagination :page="page" :count="count" :row="rows" @changePage="changePage" ></own-pagination>
    </div>

    <el-dialog :title="dialogData.title" :visible.sync="dialogFormVisible">
    <el-form>
        <el-form-item 
        v-for="(row, index) in dialogData.elements" 
        :key="row.label"
        :label="row.label" 
        :label-width="row.formLabelWidth">
            <el-input
            v-if="row.name == 'input'"
            :type="row.type"
             clearable
             style="max-width:300px"
             v-model="currentEditData[index]" 
             :placeholder="row.placeholder?row.placeholder:'请选择'"
            >

            </el-input>
            <el-select 
            v-if="row.name == 'select'" 
            :filterable="row.filterable"
            :multiple="row.multiple"
            :disabled="row.disabled"
            clearable
            :key="row.label" 
            v-model="currentEditData[index]" 
            :placeholder="row.placeholder?row.placeholder:'请选择'">
                <el-option 
                v-for="(v,i) in row.options" 
                :key="v.id" 
                :label="v.name" 
                :value="v.id"></el-option>
            </el-select>
        </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
    </div>
</el-dialog>
</div>
    
</template>

<script>


import 'element-ui/lib/theme-chalk/index.css';

import { Table, TableColumn, Button,Select,Option,Input, Dialog,Message,MessageBox,Form, FormItem } from "element-ui";

import importExcel from '^/components/importExcel.vue'
import pagination from '@/components/c-pagination.vue'


export default {
    data(){
        return {
            importExcelData:[],
            list: [],
            page: 1,
            rows: 10,
            count: 0,
            commonUrl : '',
            tableData: {
                cols : [],
                operCol:{},
            },
            
            //编辑部分
            dialogFormVisible: false,
            dialogData: {
                'title': '',
                'elements': [],
               
            },
            currentEditData: []
        }
    }, 
    props: ['propsData'],
    components : {
        'import-excel':importExcel,
        "el-table": Table,
        "el-table-column": TableColumn,
        'el-select':Select,
        'el-option': Option,
        'el-input': Input,
        "el-button": Button,
        'el-dialog': Dialog,
        'el-form': Form,
        'el-form-item': FormItem,
        'own-pagination': pagination
    },
    methods : {
        importItemsSave(){
            let _this = this
            _this.page = 1
            _this.getList()
        },
        getCols(){
            let _this = this
            _this.$axios.get(_this.commonUrl + 'getCols').then(res=>{
                _this.tableData.cols = res.data.cols
                _this.tableData.operCol = res.data.operCol,
                _this.dialogData = res.data.dialog
            })
        },
        getList(){
            let _this = this
            let param = {
                'page': _this.page,
                'rows':_this.rows
            }
            let url = window.location.origin + '/' + _this.propsData.moduleName + '/'  + _this.propsData.className + '/getList'
            _this.$axios.get(url,{params:param}).then(res=>{
                if(res.code == 1){
                    _this.list = res.data.list
                    _this.count = parseInt(res.data.count)
                }
            })
        },
        changePage(param){
            let _this = this
            _this.page = param['page']
            _this.list = []
            _this.getList()
        },
        oper(method, index, row){
            method && this[method].call(this,index,row);      
        },
        del(index, row){
                let _this = this
                MessageBox('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    _this.confirmDelete(row.id)
                }).catch(() => {
                        Message({
                            type: 'info',
                            message: '已取消删除'
                        });          
                });
       },
        edit(index, row){
            let _this = this
             _this.currentEditData = row

            _this.dialogData.elements.uid.disabled = true
            _this.dialogFormVisible = true
        },
       
        confirmDelete(id){
            let _this = this 
            _this.$axios.post(_this.commonUrl + 'foreverdelete', {'id':id}).then(res=>{
                if(res.code == 1){
                   Message({
                            type: 'success',
                            message: res.msg
                    }); 
                    _this.page = 1
                    _this.getList()
                }else{
                     Message({
                            type: 'fail',
                            message: res.msg
                    });
                }
            })
        }
    },
    created(){
        let _this = this
        _this.commonUrl = window.location.origin + '/' + _this.propsData.moduleName + '/' + _this.propsData.className + '/'
        _this.getCols()
    }
}
</script>


<style scoped>
.table{
    padding: 0 10px;
}
</style>