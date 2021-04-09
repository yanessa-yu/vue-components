<template>
        <input type="file" class="file" name="uploadfile" accept="csv, application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
     @change="importItemsSaveSon"/>
</template>

<script>

import { Toast } from 'fvmu'

const backHost = PRODUCTION?window.location.host:BACKHOST 
const uploadReportFileUrl =  'http://' + backHost + '/Public/uploadReportFile' 

export default {
    props: ['importExcelData','className','getList'],
    methods:{
         importItemsSaveSon(e){
            let _this = this;
            if(this.blockXHR){
               return
            }
            let file = e.target.files[0]
            var formData = new FormData();
            formData.append("uploadfile", file);
            $.ajax({  
                // getList=1返回数组 =0直接存入数据库 class见后台ExcelOper类uploadReportFile方法
                url: uploadReportFileUrl + "?class=" + _this.className + "&getList=" + _this.getList ,  
                type: 'post',  
                data: formData,  
                cache: false,
                processData: false,
                contentType: false,
                async: false,
                dataType: "json",
                success: function (res) {
                        if (res.code == "1") {
                            if(res.data.list && res.data.list.length > 0){
                                 _this.$emit("getImportExcelData",_this.importExcelData.concat(res.data.list))
                                 this.blockXHR = true
                                  Toast.open('上传成功')
                            }else{
                                 Toast.open('上传失败,检查上传数据是否正确')
                            }
                       }else{
                            Toast.open('上传失败')
                        }
                },
                error: function (err) {
                    Toast.open("上传表格失败,原因是"+ err.responseText +",重新提交")
                    this.blockXHR = true
                    return ;
               }
            })
        },
    }
}
</script>

<style scoped>
 .file{
    font-size: .28rem;
    padding:.3rem;
    -webkit-appearance: none;
}

</style>


