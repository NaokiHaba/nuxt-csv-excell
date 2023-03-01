<template>
  <section class="section">
    <div class="box">
      <b-field label="ファイル（.xlsx）を選択してください">
        <input type="file" @change="onFileChange"/>
      </b-field>
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
  methods: {
    onFileChange(event) {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = (event) => {
        const data = new Uint8Array(event.target.result);
        const workbook = XLSX.read(data, { type: 'array' });
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];
        const json = XLSX.utils.sheet_to_json(sheet, { header: 1 });
        console.log(json);
      };

      reader.readAsArrayBuffer(file);
    },
  },
};
</script>
