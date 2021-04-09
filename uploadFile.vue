<template>
    <div class="upload-page"  :class="{'mobile': IS_MOBILE}">           
        <form action="/" method="post"  enctype="multipart/form-data" class="upload-form">
             <div class="file-box" v-if="IS_DINGDING">上传图片
              <!-- image/x-png,image/gif,image/jpeg,image/bmp -->
                <input type="button"   class="file-btn"  @click="clickFile($event)">
            </div>
            <div class="file-box">{{ IS_DINGDING?'上传其他类型文件':'点击上传'}}
              <!-- image/x-png,image/gif,image/jpeg,image/bmp -->
                <input type="file" id="files"  class="file-btn" name="files" multiple :accept="accept" placeholder="file"  @change="tirggerFile($event)">
            </div>
        </form>

    </div>    
</template>

<script>

import Vue from 'vue'
import axios from 'axios'
import { Toast } from 'fvmu'



const backHost = PRODUCTION?window.location.host:BACKHOST 
const uploadFileUrl = 'http://' +  backHost + '/Public/uploadFileCommon?path='  //requestPayment
const getDingJsUrl = 'http://' +  backHost + '/Public/getDingJs'

export default {
        name:'uploadFile',
        props:['accept','fileList','fileListAfterUpload','savePath'],
        data() {
            return {
               IS_MOBILE:IS_MOBILE,
               IS_DINGDING: isDingding,
               dingInfo: {},
            }
        },
        methods:{
            getDingJs(){
                let _this = this
                let url = window.location.href.split('#')[0]
                $.getJSON(getDingJsUrl + '?getList=1&url=' + url)
               // axios.get('/api/Public/getDingJs?getList=1&url=' + url)
                .then(res => {
                   // res = res.data
                    if(res.code == 1){
                       _this.dingInfo = res.data.data
                       dd.config({
                        agentId: _this.dingInfo.agentId,
                        corpId: _this.dingInfo.corpId,
                        timeStamp: _this.dingInfo.timestamp,
                        nonceStr: _this.dingInfo.noncestr,
                        signature: _this.dingInfo.signature,
                        jsApiList: [
                            'biz.util.uploadImage'
                        ]
                    });
                    }
                   
                })
            },
            
            clickFile(event){
                let _this = this
                if(isDingding){
                    _this.uploadFilByDingding()
                }else{
                    _this.tirggerFile(event)
                }
            },
            tirggerFile(event){
                var _this=this;
                var list=event.target.files;   
                for(let i=0;i<list.length;i++){
                         let thisFile = list[i]
                        if(thisFile.type.indexOf('image/') >= 0 && isDingding){
                            Toast.open("图片请点击【上传图片】上传")
                            return  
                        }
                        thisFile['state'] = '0%'
                        thisFile['color'] = 'uploading'
                        _this.fileList.push(thisFile)
                        _this.uploadFil(thisFile,_this.fileList.length - 1)
                }         
            },
            uploadFilByDingding(){
                let _this = this
                dd.ready(function(){
                    dd.biz.util.uploadImage({
                        compression:true,//(是否压缩，默认为true)
                        multiple: true, //是否多选，默认false
                        max: 50, //最多可选个数
                        onSuccess : function(result) {
                             for (var k = 0; k < result.length; k++) {
                                 let thisFile = new Object();
                                 let name = result[k].split('\/')
                                 thisFile['name'] = name[name.length-1]
                                 thisFile['state'] = '上传成功'
                                 thisFile['color'] = 'success'
                                 _this.fileListAfterUpload.push(result[k])
                                 _this.fileList.push(thisFile)
                                
                             }
                        },
                        onFail : function(result) {
                            if(arguments[0]!='backdrop click'){
                                dd.device.notification.toast({
                                    text: "程序启动失败" //提示信息
                                })
                            }
                        }
                    })
                });
                dd.error(function(err) {
                    console.log('dd error: ' + JSON.stringify(err));
                });
            },    
            uploadFil(file,thisIndex){
                let _this=this;
                var formData=new FormData();
                formData.append('file',file);
                $.ajax({
                    url: uploadFileUrl + _this.savePath,
                    type: 'POST',
                    data: formData,
                    dataType:"json",
                    async: true,
                    cache: false,
                    contentType: false,//不要设置Content-Type请求头，因为文件数据是以 multipart/form-data 来编码
                    processData: false, //不要对data参数进行序列化处理，默认为true
                    beforeSend: function(xhr) {
                    },
                    xhr: function(){
                        let myXhr = $.ajaxSettings.xhr();
                        if(myXhr.upload){
                            myXhr.upload.addEventListener('progress',function(e) {
                                if (e.lengthComputable) {
                                    var percent = Math.floor(e.loaded/e.total*100);
                                    if(percent < 100) {
                                        file['state'] = percent+'%'
                                        _this.fileList.splice(thisIndex, 1, file)
                                    }
                                    if(percent >= 100) {
                                        file['state'] = '上传成功'
                                        file['color'] = 'success'
                                        _this.fileList.splice(thisIndex, 1, file)
                                    }
                                }
                            }, false);
                        }
                        return myXhr;
                    },
                    success: function (data) {
                        if(data.code == 1){
                          _this.fileListAfterUpload.push(data.msg);
                          //_this.$emit('input',_this.fileListAfterUpload) //传递值给父组件
                        }else{
                            file['state'] = '上传失败'
                            file['color'] = 'fail'
                            _this.fileList.splice(thisIndex, 1, file)
                        }
                    },
                    error: function (data) {
                        file['state'] = '上传失败'
                        file ['color'] = 'fail'
                        _this.fileList.splice(thisIndex, 1, file)
                    }
                });
            },
        },
        created: function(){
            let _this = this
            _this.getDingJs()
        }
    }
</script>
<style scoped>
.upload-page{margin-top: .2rem;padding: 0 .1rem;border-top: .01rem solid #ddd;}
.mobile{width: 100%;}
.upload-form{margin: .2rem ;}
.file-box{
    padding:5px 15px;
    border-radius:3px;
    background-color:#409eff;
    border-color:#409eff;
    color:#fff;display:inline-block;position: relative;
}
.file-btn{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    outline: none;
    background-color: transparent;
    filter:alpha(opacity=0);
    -moz-opacity:0;
    -khtml-opacity: 0;
    opacity: 0;
}


</style>

