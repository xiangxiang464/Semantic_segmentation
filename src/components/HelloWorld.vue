<script setup lang="ts">
  import { ref } from 'vue'
  import { ElMessage } from 'element-plus'
  import { Plus } from '@element-plus/icons-vue'

  import type { UploadProps } from 'element-plus'
  import { type } from 'os'

  const imageUrl = ref('')
  const seg_url = ref('')
  const loading = ref(false)
  const activeName = ref('0')
  const choice = ref(0)
  const data = {"num":choice.value}
  const tableData = ref([])

  const handleChange = (name: string) => {
    console.log(name,typeof(name))  
    choice.value = Number(name)
  }
  const handleAvatarSuccess: UploadProps['onSuccess'] = (
    response,
    uploadFile
  ) => {
    imageUrl.value = URL.createObjectURL(uploadFile.raw!)
    seg_url.value = 'data:image/jpeg;base64,' + response.image_base64;
    // eval = response.eval
    loading.value = false
  }
  const handleError: UploadProps['onSuccess'] = (error) => {
    ElMessage.error('上传失败');
    loading.value = false
  }
  const beforeAvatarUpload: UploadProps['beforeUpload'] = (rawFile) => {
    imageUrl.value = URL.createObjectURL(rawFile);
    loading.value = true
    return true
  }
  const indexMethod = (index: number) => {
    const kpi = ['IOU','ACC']
    return kpi[index]
  }
</script>

<template>
  <div class="demo-image">
    <el-tabs v-model="activeName" class="demo-tabs" @tab-change="handleChange">
      <el-tab-pane label="potsdam" name="0">User</el-tab-pane>
      <el-tab-pane label="vaihingen" name="1">Config</el-tab-pane>
    </el-tabs>
    <div class="image-list">
      <el-upload
        class="avatar-uploader"
        action="http://127.0.0.1:5000/upload"
        :data=data
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :on-error="handleError"
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
    <el-table :data="tableData" style="margin-top:24px; width:100%">
      <el-table-column type="index" :index="indexMethod" />
      <el-table-column prop="impervious_surface" label="Impervious_surface" />
      <el-table-column prop="building" label="building"  />
      <el-table-column prop="low_vegetation" label="low_vegetation"  />
      <el-table-column prop="tree" label="tree"  />
      <el-table-column prop="car" label="car"  />
      <el-table-column prop="clutter" label="clutter" />
    </el-table>
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
    border: 1px solid black;
  }
  .img_placeholder {
    width: 512px;
    height: 512px;
    margin-left: 5%;
    border: 1px solid black;
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
  .demo-tabs{
    margin-left: 3%;
    margin-right: 3%;
  }

</style>
<style>
  .avatar-uploader .ep-upload {
    border: 1px dashed black;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    margin-right: 5%;
  }

  .demo-tabs > .ep-tabs__content {
    padding: 24px;
    color: #6b778c;
    font-size: 32px;
    font-weight: 600;
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