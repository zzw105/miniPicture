<template>
  <iframe src="" width="200" height="200" frameborder="0" name="myFrame" style="display: NONE"></iframe>
  <div class="mainBox">
    <div class="titleImg"><img src="/img/1.png" alt="" /></div>
  </div>
  <input ref="file" type="file" name="logo" />
  <input type="button" value="提交" @click="up" />
  <img v-if="minUrl" :src="minUrl" alt="" />
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const file = ref<HTMLInputElement>()
const minUrl = ref('')
const up = async () => {
  const formData = new FormData()
  const fileData = file.value?.files && file.value?.files[0]
  if (fileData) {
    formData.append('file', fileData)
  }

  const res = await axios.post('https://node.zzw105.com/upFile', formData, {
    // const res = await axios.post('http://127.0.01:1111/upFile', formData, {
    // 因为我们上传了图片,因此需要单独执行请求头的Content-Type
    headers: {
      // 表示上传的是文件,而不是普通的表单数据
      'Content-Type': 'multipart/form-data'
    }
  })

  if (res.data.code === 200) {
    minUrl.value = '/miniPicture/minFiles/' + res.data.data
  }
}
</script>

<style scoped>
.titleImg {
  display: flex;
  width: 300px;
  height: 300px;
}
</style>
<style></style>
