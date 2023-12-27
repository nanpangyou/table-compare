<script setup>
import { ref } from "vue";
import { UploadFilled } from "@element-plus/icons-vue";
import { read, utils, writeFile, readFile } from "xlsx";
import XLSX from "xlsx";
const uploadRef = ref();

const submitUpload = () => {
  console.log(uploadRef.value);
};
const onFileChange = async (file) => {
  console.log(1111111111);
  console.log(file);
  uploadRef.value = file;
  const reader = new FileReader();
  reader.readAsBinaryString(file.raw);
  reader.onload = (evt) => {
    const data = evt.target.result;
    const wb = XLSX.read(data, { type: "binary" });
    console.log(wb);
  };
};
</script>

<template>
  <el-card class="box-card">
    <el-alert title="第一步，上传当日申请单" type="success" :closable="false" />
    <el-upload
      class="upload-demo"
      :auto-upload="false"
      ref="uploadRef"
      :onChange="onFileChange"
      drag
    >
      <el-icon class="el-icon--upload"><upload-filled /></el-icon>
      <div class="el-upload__text">
        Drop file here or <em>click to upload</em>
      </div>
      <template #tip>
        <div class="el-upload__tip">选当天的申请表</div>
      </template>
    </el-upload>
    <el-button class="ml-3" type="success" @click="submitUpload">
      upload to server
    </el-button>
  </el-card>
</template>
