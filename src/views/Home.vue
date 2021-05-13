<template>
  <div class="home">
    <h1>
      FBA 货件生成器
    </h1>

    <el-input style="width: 700px;margin-bottom: 30px;" v-model="input" placeholder="planName"></el-input>
    <div style="margin-bottom: 30px;">
      <el-select v-model="value" placeholder="请选择发货地址">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>
    </div>

    <div style="display: flex;justify-content: center;">
      <el-input style='width:200px; margin-right: 50px;' type="textarea" autosize placeholder="EAN"
        :autosize="{ minRows: 2, maxRows: 4}" v-model="textarea1"></el-input>
      <div style="margin: 20px 0;"></div>
      <el-input style="width:200px" type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="Quananity"
        v-model="textarea2"></el-input>

    </div>
    
    <el-button @click="senddata" style="margin-top: 20px;" type="primary">生成FBA plan</el-button>
    <el-button @click='toNext' style="margin-top: 20px;" type="primary">生成外箱尺寸表格</el-button>

  </div>
</template>

<script>
  export default {
    name: "app",
    components: {
    },
    data() {
      return {
        options: [{
          value: 'hangzhou',
          label: '杭州办公室'
        },{
          value: 'oversea',
          label: '雄达海外仓'
        }],
        value: '',
        input: '',
        textarea1: '',
        textarea2: ''
      }
    },
    methods: {
      toNext() {
        this.$router.push('index')
      },
      senddata() {
        console.log(this.$data.textarea1)
        console.log(this.$data.textarea2)
        this.$axios({
          method:'post',
          url:'/fbaplan',
          data:{
            addr:this.$data.value,
            planname:this.$data.input,
            amount:this.$data.textarea2,
            eanlist:this.$data.textarea1
          }
        })
        .then((res) => {
          const link = document.createElement('a')
          let blob = new Blob([res.data], { type: "text/plain" })
          let objectUrl = URL.createObjectURL(blob) // 创建URL
          link.href = objectUrl
          link.download = 'shipment.txt' // 自定义文件名
          link.click() // 下载文件
          URL.revokeObjectURL(objectUrl); // 释放内存
        }).catch((error) =>
          console.log(error)       //请求失败返回的数据
        )
      }
    }
  };





</script>