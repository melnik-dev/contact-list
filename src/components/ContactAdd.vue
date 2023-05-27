<template>
  <div class="change-box">
    <input v-model="name" class="change-input" type="text" placeholder="Имя">
    <input v-model="surname" class="change-input" type="text" placeholder="Фамилия">
    <input v-model="phone" class="change-input" type="text" placeholder="Телефон">
    <input v-model="mail" class="change-input" type="text" placeholder="E-mail">

    <slot name="actions"
          :onAddContact="onAddContact"
          :onChangeContact="onChangeContact"
          :onCancel="onCancel">
      <button class="btn btn-small" @click="onAddContact">
        Добавить
      </button>
    </slot>
    <span :class="['change__note', { 'change__note--none' : note }]">Не все поля заполнены</span>
  </div>
</template>

<script>
export default {
  name: "ContactAdd",
  props: {
    contact: Object
  },
  emits: ['onAddContact', 'onChangeContact', 'onCancel'],
  data() {
    return {
      name: '',
      surname: '',
      phone: '',
      mail: '',
      note: true
    }
  },
  methods: {
    onAddContact() {
      if (this.name === '' &&
          this.surname === '' &&
          this.phone === '' &&
          this.mail === '') {
        this.note = false
        return
      }
      this.$emit('onAddContact', {
        name: this.name,
        surname: this.surname,
        phone: this.phone,
        mail: this.mail
      })
      this.name = ''
      this.surname = ''
      this.phone = ''
      this.mail = ''
      this.note = true
    },
    onChangeContact() {
      this.$emit('onChangeContact', {
        id: this.contact.id,
        name: this.name,
        surname: this.surname,
        phone: this.phone,
        mail: this.mail
      })
    },
    onCancel() {
      this.$emit('onCancel')
    }
  },
  mounted() {
    if (this.contact) {
      this.name = this.contact.name
      this.surname = this.contact.surname
      this.phone = this.contact.phone
      this.mail = this.contact.mail
    }
  }
}
</script>

<style scoped>
.change-box {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5px;
  margin-bottom: 10px;
}

.change-input {
  padding: 5px;
  border: 1px solid black;
  border-radius: 4px;
}

.change__note {
  display: flex;
  align-items: center;
  justify-content: center;
  color: red;
}

.change__note--none {
  display: none;
}
</style>