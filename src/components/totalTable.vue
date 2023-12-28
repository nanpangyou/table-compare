<script setup>
import { ref, reactive } from "vue";
import { UploadFilled } from "@element-plus/icons-vue";
import { read, utils, writeFile, readFile } from "xlsx";
import XLSX from "xlsx";
let column = reactive({ columns: [] });
const sheetName = reactive({ sheetName: [] });
const selectedSheet = ref("");
const tableData = ref({});

const onFileChange = (file) => {
  handleExcelData(file);
};
const handleExcelData = async (file) => {
  const data = await getExcelData(file);
  console.log("data", data);
  tableData.value = data;
  getAllSheets(data);
};

const getAllSheets = (data) => {
  sheetName.sheetName = data.SheetNames;
};
const handleSelectChange = (data) => {
  getSelectedSheetColumn(data);
};
const getSelectedSheetColumn = (sheetName) => {
  const json = utils.sheet_to_json(tableData.value.Sheets[sheetName]);
  console.log("json", sheetName, json);
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
    <el-alert title="第二步，上传总表" type="success" :closable="false" />
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
        <div class="el-upload__tip">选总表</div>
      </template>
    </el-upload>

    <div>{{ column }}</div>
    <div>{{ sheetName.sheetName }}</div>
    <el-select
      v-model="selectedSheet"
      class="m-2"
      placeholder="请选择sheet表格"
      @change="handleSelectChange"
    >
      <el-option
        v-for="item in sheetName.sheetName"
        :key="item"
        :label="item"
        :value="item"
      />
    </el-select>
  </el-card>
</template>
