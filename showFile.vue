<template>
    <table class="upload-list">
            <tr v-for="(file, index) in fileList" class="upload-list-tr">
              <td><a href="#" class="upload-list-td upload-list-td-name" @click="previewFile(index)">{{IS_MOBILE && file.name.length > 15?file.name.substr(0,15) + '....' + file.name.split('.')[1] :file.name}}</a></td>
              <td><span class="upload-list-td upload-list-td-state" :class="file.color">{{ file.state }}</span> </td>
              <td v-if="file.noDel == undefined">
                  <div class="upload-list-td upload-list-td-del" @click="deleFile(index)">
                    <span class="icon icon-delete"><svg><use xlink:href="#delete" /></svg></span> 
                  </div>
              </td>
            </tr>
     </table>
</template>

<script>
import star from '@/assets/images/delete.svg'
import { Toast } from 'fvmu'

const backHost = PRODUCTION?window.location.host:BACKHOST 
const delCircleDingdingVideoUrl = 'http://' +  backHost + '/Public/delCircleDingdingVideo'


export default {
    name:'showFile',
    data:function(){
        return({
            IS_MOBILE:IS_MOBILE,
            IS_DINGDING: isDingding,
        })
    },
    props:['fileList','fileListAfterUpload'],
    methods:{
        deleFile(i){
                let _this = this
                $.getJSON(delCircleDingdingVideoUrl + '?file_path=' +  this.fileListAfterUpload[i])
                .then(res => {
                    if(res.code == 1){
                        Toast.open("删除成功")
                        this.fileList.splice(i,1);
                        this.fileListAfterUpload.splice(i,1)
                    }else{
                        Toast.open("删除失败")
                    }
                })
            }, 
            previewFile(i){
               let _this = this
               if(_this.fileListAfterUpload[i].indexOf('static.dingtalk.com') >= 0){
                   let url = _this.fileListAfterUpload[i]
                   if( isDingding){
                        _this.showPicBase([url],0)
                    }
               }else{
                   let url = 'http://' + backHost + _this.fileListAfterUpload[i]
                   dd.biz.util.openLink({
                            url:url,
                            onSuccess : function(result) {
                            },
                            onFail : function() {}
                        })
               }
            }, 
            showPicBase: function(images_list,index){
                dd.biz.util.previewImage({
                    urls: images_list,//图片地址列表
                    current: images_list[index],//当前显示的图片链接
                    onSuccess : function(result) {
                       console.log(result)
                    },
                    onFail : function(err) {
                        console.log(err)
                    }
                })
            }, 
    }
}
</script>

<style scoped>
    .upload-list{margin: .2rem; }
    .upload-list-tr{
        line-height: 3;
    }
    .upload-list-tr:hover{background-color: #f5f7fa;}
    .upload-list-td{padding-right: .8rem;}
    .upload-list-td-name{
        width: 20%;
    }
    .fail{color: red;}
    .success{color: green;}
    .icon{
        display: inline-block;
        width: .25rem;
        height: .25rem;
    }
    .icon-preview{
        width: .3rem;
        height: .3rem;
    }
    .icon svg{
        width: 100%;
        height: 100%;
    }
</style>
