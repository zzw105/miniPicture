<template>
  <div class="mainBox">
    <div class="titleImg">
      <img src="/img/1.png" alt="" />
      <div class="text">小表情生成器</div>
      <img src="/img/1.png" alt="" />
    </div>
    <div class="selectPicture">
      <a-radio-group v-model:value="size" :disabled="minUrl !== ''" option-type="button" :options="options" />
      <div class="selectPictureBox">
        <img v-if="minUrl" :src="minUrl" alt="" />
        <div v-else @click="up">请选择图片</div>
      </div>
      <a-button :disabled="!minUrl" type="primary" @click="clean">重新选择图片</a-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { message } from 'ant-design-vue'
import { onMounted, ref } from 'vue'
import axios from 'axios'

const file = ref<HTMLInputElement>(document.createElement('input'))
const minUrl = ref('')
const size = ref('80')
const options = [
  { label: '特小号', value: '60' },
  { label: '小号', value: '80' },
  { label: '中号', value: '100' }
]
const up = async () => {
  file.value.click()
  file.value.onchange = async () => {
    const formData = new FormData()
    const fileData = file.value.files && file.value.files[0]

    if (!fileData) {
      message.error('未检测到文件')
      return
    }

    // 条件过滤
    const typeList = ['png', 'jpg,', 'jpeg']
    if (!typeList.find((item) => fileData.type.includes(item))) {
      message.error('请选择图片类文件')
      return
    }

    if (fileData.size > 10 * 1024 * 1024) {
      message.error('图片文件不允许超过10M')
      return
    }

    formData.append('file', fileData)
    formData.append('size', size.value)

    // 发送请求
    const res = await axios.post('https://node.zzw105.com/upFile', formData, {
      // const res = await axios.post('http://127.0.0.1:1111/upFile', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    })

    // 响应处理
    if (res.data.code === 200) {
      minUrl.value = '/miniPicture/minFiles/' + res.data.data
      console.log(message)
      message.success('转换完成')
    } else {
      message.error('转换失败')
    }
  }
}

const clean = () => {
  file.value.value = ''
  file.value.files = null
  minUrl.value = ''
}

// 初始化
onMounted(() => {
  file.value.type = 'file'
  file.value.name = 'logo'
  file.value.accept = '.png, .jpg, .jpeg'
})
</script>

<style scoped lang="less">
.titleImg {
  display: flex;
  width: 100%;
  height: 80px;
  align-items: center;
  justify-content: space-evenly;
  margin-bottom: 10px;
  background-color: rgb(211, 211, 211);
  .text {
    font-size: 20px;
  }

  img {
    height: 70px;
  }
}
.selectPicture {
  display: flex;
  width: 100%;
  height: 200px;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  .selectPictureBox {
    display: flex;
    width: 200px;
    height: 100%;
    align-items: center;
    justify-content: space-evenly;
    color: rgb(155, 155, 155);
    border: 2px dotted rgb(155, 155, 155);
    margin-bottom: 10px;
    margin-top: 10px;

    div {
      display: flex;
      width: 100%;
      height: 100%;
      align-items: center;
      justify-content: space-evenly;
    }
  }
}
</style>
<style>
html,
body,
#app {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
</style>
