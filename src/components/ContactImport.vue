<template>
  <label>
    <span class="btn btn-small">Импорт</span>
    <input class="upload-input" type="file" @change="onUpload">
  </label>
</template>

<script>
export default {
  name: "ContactImport",
  emits: ['onUploadFile'],
  methods: {
    parseCSV(csvData) {
      const rows = csvData.split("\n"); // Разбиваем CSV-строку на строки

      const header = rows[0].split(","); // Получаем заголовки столбцов из первой строки
      const data = [];

      for (let i = 1; i < rows.length; i++) {
        const values = rows[i].split(","); // Разбиваем строку на значения


        const object = {};
        for (let j = 0; j < header.length; j++) {
          object[header[j].trim()] = values[j]; // Создаем свойство объекта с использованием заголовка и значения
        }
        data.push(object);

      }

      return data;
    },
    onUpload(event) {
      const file = event.target.files[0]
      const reader = new FileReader()
      console.log(file)
      if (file) {
        reader.readAsText(file)

        reader.onload = () => {
          this.parse = reader.result
          if (reader.result) {
            this.$emit('onUploadFile', this.parseCSV(reader.result))
          }
        }
        reader.onerror = () => {
          console.log(reader.error)
        }
      } else {
        console.log('файл не выбран')
      }


    }
  }
}
</script>

<style scoped>
.upload-input {
  display: none;
}
</style>