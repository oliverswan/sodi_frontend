<template>
<div>
  <uploader :options="options" class="uploader-example">
    <uploader-unsupport></uploader-unsupport>
    <uploader-drop>
      <p>Drop files here to upload</p>
      <uploader-btn>select files</uploader-btn>
      <uploader-btn :attrs="attrs">select images</uploader-btn>
      <uploader-btn :directory="true">select folder</uploader-btn>
    </uploader-drop>
    <uploader-list></uploader-list>
  </uploader> 
    <!-- <uploader :options="options2" class="uploader-example">
    <uploader-unsupport></uploader-unsupport>
    <uploader-drop>
      <p>Drop files here to upload price list</p>
      
    </uploader-drop>
    <uploader-list></uploader-list>
  </uploader> -->
  <input type="file" value="" id="file" @change="uploadConfig">
  </div>
</template>

<script>
import API from '../../api/config'
import axios from 'axios';
  export default {
    data () {
      return {
        options: {
          // https://github.com/simple-uploader/Uploader/tree/develop/samples/Node.js
          target: API.host+'/upload/invoices',
          testChunks: false
        },
        options2: {
          target: API.host+'/upload/prices',
          testChunks: false
        },
        attrs: {
          accept: 'image/*',
          singleFile:false
        }
      }
    },
     methods: {
    uploadConfig(e) {
        let formData = new FormData();
        formData.append('file', e.target.files[0]);
        let url = API.host+'/upload/prices';
        let config = {
          headers:{'Content-Type':'multipart/form-data'},
          responseType: 'blob'
        };
       axios.post(url,formData, config).then(function (response) {
          console.log(response.data)

          const content = response.data;
                        const blob = new Blob([content]);
                        const fileName = `price.csv`;
                        if ("download" in document.createElement("a")) {
                            // 非IE下载
                            const elink = document.createElement("a");
                            elink.download = fileName;
                            elink.style.display = "none";
                            elink.href = URL.createObjectURL(blob);
                            document.body.appendChild(elink);
                            elink.click();
                            URL.revokeObjectURL(elink.href); // 释放URL 对象
                            document.body.removeChild(elink);
                        } else {
                            // IE10+下载
                            navigator.msSaveBlob(blob, fileName);
                        }
        });

//         axios({
//     method: 'post',
//     url: url,
//     formData,
//     responseType: 'blob'
// }).then(response => {
//     // this.download(response)
// }).catch((error) => {});

      }
     }
  }
</script>

<style>
  .uploader-example {
    width: 880px;
    padding: 15px;
    margin: 40px auto 0;
    font-size: 12px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .4);
  }
  .uploader-example .uploader-btn {
    margin-right: 4px;
  }
  .uploader-example .uploader-list {
    max-height: 440px;
    overflow: auto;
    overflow-x: hidden;
    overflow-y: auto;
  }
</style>
