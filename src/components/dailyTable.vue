<script setup>
import { ref, reactive } from "vue";
import { UploadFilled } from "@element-plus/icons-vue";
import { read, utils, writeFile, readFile } from "xlsx";
import XLSX from "xlsx";
let column = reactive({ columns: [] });

const onFileChange = (file) => {
  handleExcelData(file);
};
const handleExcelData = async (file) => {
  const data = await getExcelData(file);
  console.log("data", data);
  getAllColumn(data);
};
const getAllColumn = (data) => {
  const json = utils.sheet_to_json(data.Sheets[data.SheetNames[0]]);
  console.log("json", json);
  column.columns = Object.keys(json[0]);
};
const getExcelData = (file) =>
  new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.readAsBinaryString(file.raw);
    reader.onload = (evt) => {
      const data = evt.target.result;
      const wb = read(data, { type: "binary" });
      wb ? resolve(wb) : reject("error");
    };
  });
</script>

<template>
  <el-card class="box-card">
    <el-alert title="第一步，上传当日申请单" type="success" :closable="false" />
    <el-upload
      class="upload-demo"
      :auto-upload="false"
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

    <div>{{ column }}</div>
  </el-card>
</template>
