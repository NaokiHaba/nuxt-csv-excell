<template>
  <section class="section">
    <div class="box">
      <b-field label="ファイル（.xlsx）を選択してください">
        <input type="file" @change="onFileChange"/>
      </b-field>
    </div>
    <div>
      <button @click="exportCsv">CSVエクスポート</button>
    </div>
  </section>
</template>

<script>
import Vue from 'vue'
import Buefy from 'buefy'
import 'buefy/dist/buefy.css'

Vue.use(Buefy)

import * as XLSX from "xlsx";

export default {
  data() {
    return {
      items: [
        {name: "Alice", age: 20},
        {name: "Bob", age: 25},
        {name: "Charlie", age: 30}
      ]
    };
  },
  methods: {
    exportCsv() {
      // 1. CSVデータを取得するための配列を用意する
      const csvData = this.items.map(item => `${item.name},${item.age}`);

      // 2. CSV形式に変換するための関数を定義する
      const csvString = "名前,年齢\n" + csvData.join("\n");

      // 3. CSVファイルをダウンロードする
      const link = document.createElement("a");
      link.href = "data:text/csv;charset=utf-8," + encodeURIComponent(csvString);
      link.download = "export.csv";
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    onFileChange(event) {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = (event) => {
        const data = new Uint8Array(event.target.result);
        const workbook = XLSX.read(data, {type: 'array'});
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
        const json = XLSX.utils.sheet_to_json(sheet, {header: 1});
        console.log(json);
      };

      reader.readAsArrayBuffer(file);
    },
  },
};
</script>
