<template>
  <div id="app">
    <div class="form">
      <div class="form-item">
        <div class="label">上传地址:</div>
        <div class="content">
          <input v-model="options.target" />
        </div>
      </div>
      <div class="form-item">
        <div class="label">分片大小:</div>
        <div class="content">
          <input type="number" v-model="mbSize" style="padding-right: 50px;" />
          <span class="unit">MB</span>
        </div>
      </div>
      <div class="form-item">
        <div class="label">分片重试次数:</div>
        <div class="content">
          <input type="number" v-model="options.maxChunkRetries" />
        </div>
      </div>
    </div>
    <uploader :options="options" :file-status-text="statusText" class="uploader-example" ref="uploader" @file-complete="fileComplete" @complete="complete">
      <uploader-unsupport></uploader-unsupport>
      <uploader-drop>
        <p>Drop files here to upload or</p>
        <uploader-btn>select files</uploader-btn>
        <uploader-btn :attrs="attrs">select images</uploader-btn>
        <uploader-btn :directory="true">select folder</uploader-btn>
      </uploader-drop>
      <uploader-list></uploader-list>
    </uploader>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      mbSize: 1,
      options: {
        target: 'http://localhost:3000/upload',
        maxChunkRetries: 0,
        chunkSize: 0,
        headers: {}
      },
      attrs: {
        accept: 'image/*'
      },
      statusText: {
        success: '成功了',
        error: '出错了',
        uploading: '上传中',
        paused: '暂停中',
        waiting: '等待中'
      }
    }
  },
  watch: {
    mbSize: {
      handler(newValue) {
        this.options.chunkSize = newValue * 1024 * 1024
      },
      immediate: true
    },
    options: {
      handler(newOptions) {
        Object.assign(this.$refs.uploader.uploader.opts, newOptions)
      },
      deep: true
    }
  },
  methods: {
    complete () {
      console.log('complete', arguments)
    },
    fileComplete () {
      console.log('file complete', arguments)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
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
  .form{
    width: 600px;
    margin: 0 auto;
  }
  .form .form-item +.form-item{
    margin-top: 10px;
  }
  .form .form-item .label{
    width: 120px;
    text-align: right;
    float: left;
    line-height: 31px;
  }
  .form .form-item .content{
    margin-left: 120px;
    position: relative;
  }
  .form .form-item .content input{
    box-sizing: border-box;
    margin: 0;
    padding: 6px 8px;
    width: 100%;
  }
  .form .form-item .content .unit{
    position: absolute;
    right: 4px;
    top: 50%;
    transform: translateY(-50%);
  }
</style>
