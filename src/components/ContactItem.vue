<template>
  <div>
    <div v-if="!isChange" class="contact__box">
      <div class="contact__info">
        <h3>{{ contact.name }} {{ contact.surname }}</h3>
        <p>{{ contact.phone }}</p>
        <p>{{ contact.mail }}</p>
      </div>
      <div class="contact__btn-box">
        <button @click="onActivateChange" class="btn btn-small">Изменить</button>
        <button @click="onRemoveContact(contact.id)" class="btn btn-small">Удалить</button>
      </div>
    </div>
    <ContactAdd v-else
        :contact="contact"
        @onChangeContact="onChangeContact"
        @onCancel="onCancel">
      <template #actions="{ onChangeContact, onCancel }">
        <button class="btn btn-small" @click="onChangeContact">
          Изменить
        </button>
        <button class="btn btn-small" @click="onCancel">
          Отмена
        </button>
      </template>
    </ContactAdd>
  </div>

</template>

<script>
import ContactAdd from './ContactAdd.vue'

export default {
  name: "ContactItem",
  components: {
    ContactAdd,
  },
  props: {
    contact: Object
  },
  emits: ['onRemoveContact', 'onChangeContact', 'onCancel'],
  data() {
    return {
      isChange: false
    }
  },
  methods: {
    onRemoveContact(id) {
      this.$emit('onRemoveContact', id)
    },
    onActivateChange() {
      this.isChange = true
    },
    onChangeContact(payload) {
      this.$emit('onChangeContact', payload)
      this.onCancel()
    },
    onCancel() {
      this.isChange = false
    }
  }
}
</script>

<style scoped>
.contact__box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}
.contact__btn-box {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
</style>