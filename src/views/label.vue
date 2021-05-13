<template>
  <div>
    <h1>打印内箱标标签</h1>
    <h2>内箱标签下载
      <el-button type="primary" @click='get_label' icon="el-icon-download" circle></el-button>
    </h2>
    <h2>外箱尺寸表单下载
      <el-button type="primary" @click='get_box_config' icon="el-icon-download" circle></el-button>
    </h2>
  </div>
</template>
<script>
  export default {
    name: "app",
    components: {
    },
    data() {
      return {
        baseurl:'http://locolhost:1099/'
      }
    },
    methods: {
      get_label() {
        const link = document.createElement('a')
        link.href = this.baseurl+'/label'
          link.download = 'shipment.xlsx' // 自定义文件名
          link.click() // 下载文件
      },
      get_box_config() {
        const link = document.createElement('a')
        link.href =this.baseurl+ '/ship'
          link.download = 'shipment.xlsx' // 自定义文件名
          link.click() // 下载文件
      }

    }
  };





</script>