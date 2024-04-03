<script setup lang="ts">
  import { ref } from 'vue'
  import { ElMessage } from 'element-plus'
  import { Plus } from '@element-plus/icons-vue'

  import type { UploadProps } from 'element-plus'

  const imageUrl = ref('')
  const seg_url = ref('')
  const loading = ref(false)
  const handleAvatarSuccess: UploadProps['onSuccess'] = (
    response,
    uploadFile
  ) => {
    imageUrl.value = URL.createObjectURL(uploadFile.raw!)
    seg_url.value = 'data:image/jpeg;base64,' + response.image_base64;
    loading.value = false
  }

  const beforeAvatarUpload: UploadProps['beforeUpload'] = (rawFile) => {
    imageUrl.value = URL.createObjectURL(rawFile);
    loading.value = true
    return true
  }
</script>

<template>
  <div class="demo-image">
    <div class="image-list">
      <el-upload
        class="avatar-uploader"
        action="http://127.0.0.1:5000/upload"
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :before-upload="beforeAvatarUpload"
      >
        <img v-if="imageUrl" :src="imageUrl" class="avatar" />
        <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
      </el-upload>
      <el-image class="img_style" v-if="seg_url" :src="seg_url"  />
      <div v-else-if="imageUrl" class="img_loading" v-loading="loading"
       element-loading-background="rgba(122, 122, 122, 0.8)"></div>
      <div v-else class="img_placeholder"></div>
    </div>
  </div>
</template>

<style scoped>
  .demo-image .block {
    padding: 30px 0;
    text-align: center;
    display: inline-block;
    width: 20%;
    box-sizing: border-box;
    vertical-align: top;
  }

  .image-list{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
  .avatar-uploader .avatar {
    width: 512px;
    height: 512px;
    display: block;
  }
  .img_style {
    width: 512px; 
    height: 512px;
    margin-left: 5%;
  }
  .img_placeholder {
    width: 512px;
    height: 512px;
    margin-left: 5%;
    border: 1px solid black;
    border-radius: 12px;
  }
  .img_loading {
    width: 512px;
    height: 512px;
    margin-left: 5%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f5f5f5; /* 加载背景颜色 */
  }

</style>
<style>
  .avatar-uploader .ep-upload {
    border: 1px dashed black;
    border-radius: 12px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    margin-right: 5%;
  }

  .avatar-uploader .ep-upload:hover {
    border-color: blue;
  }

  .ep-icon.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 512px; 
    height: 512px;
    text-align: center;
  }
</style>