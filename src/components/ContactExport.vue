<template>
  <a @click="onDownload" download="contacts.csv" class="btn btn-small btn-download" href='#'>Экспорт</a>
</template>

<script>
export default {
  name: "ContactExport",
  props: {
    contactList: Array
  },
  methods: {
    convertToCSV() {
      const csvHeader = Object.keys(this.contactList[0]).join(",") // заголовки столбцов из первого объекта массива

      const csvRows = this.contactList.map((object) =>
          Object.values(object).map((value) => `"${value}"`).join(",")) // Преобразуем каждый объект в строку

      return [csvHeader, ...csvRows].join("\n")
    },
    onDownload(event) {
      const blob = new Blob([this.convertToCSV()], {type: "text/csv;charset=utf-8;"})
      event.currentTarget.href = URL.createObjectURL(blob)
    },
  }
}
</script>

<style scoped>
.btn-download {
  margin-right: 10px;
  text-decoration: none;
  color: black;
}
</style>