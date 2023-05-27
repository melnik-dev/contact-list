<template>
  <div class="search-filter__box">
    <div class="btn__box">
      <button class="btn" @click="onActiveAdd">Добавить</button>
      <button class="btn" @click="onSearchUser">Найти</button>
    </div>
    <ContactAdd
        v-if="isActiveAdd"
        @onAddContact="onAddContact"/>
    <ContactSearch
        v-model="searchValue"
        v-else/>
  </div>
  <div class="contact__list">
    <ContactItem
        v-for="contact in filterContacts"
        :key="contact.id"
        :contact="contact"
        @onRemoveContact="onRemoveContact"
        @onChangeContact="onChangeContact"/>
  </div>
  <div class="load__box">
    <ContactExport :contact-list="contactList"/>
    <ContactImport @onUploadFile="onUploadFile"/>
  </div>

</template>

<script>
import ContactSearch from './components/ContactSearch.vue'
import ContactAdd from './components/ContactAdd.vue'
import ContactItem from './components/ContactItem.vue'
import ContactExport from './components/ContactExport.vue'
import ContactImport from './components/ContactImport.vue'

export default {
  name: 'App',
  components: {
    ContactSearch,
    ContactAdd,
    ContactItem,
    ContactExport,
    ContactImport
  },
  data() {
    return {
      isActiveAdd: true,
      contactList: [{
        id: 1,
        name: 'Elon',
        surname: 'Musk',
        phone: '12345',
        mail: 'Musk@mail.com'
      }, {
        id: 2,
        name: 'Aleks',
        surname: 'Mans',
        phone: '12345',
        mail: 'Musk@mail.com'
      }],
      searchValue: '',
      parse: []
    }
  },
  methods: {
    onActiveAdd() {
      this.isActiveAdd = true
    },
    onSearchUser() {
      this.isActiveAdd = false
    },
    onAddContact(payload) {
      console.log(payload)
      const newContact = {
        id: this.contactList.length + 1,
        name: payload.name,
        surname: payload.surname,
        phone: payload.phone,
        mail: payload.mail,
      }
      this.contactList.push(newContact)
    },
    onRemoveContact(id) {
      console.log(id)
      this.contactList = this.contactList.filter(c => c.id !== id)
    },
    onChangeContact(payload) {
      console.log(payload)
      this.contactList.map(c => {
        if (c.id === payload.id) {
          c.name = payload.name
          c.surname = payload.surname
          c.phone = payload.phone
          c.mail = payload.mail
        }
      });
    },
    onUploadFile(payload) {
      console.log(payload)
      this.contactList = payload
    }
  },
  computed: {
    filterContacts() {
      return this.contactList.filter(с => {
            return с.name.toLowerCase().includes(this.searchValue.toLowerCase()) || с.surname.toLowerCase().includes(this.searchValue.toLowerCase())
          }) || []
    }
  },
  watch: {
    contactList: {
      handler() {
        window.localStorage.setItem('contactList', JSON.stringify(this.contactList))
      },
      deep: true
    }
  },
  mounted() {
    this.contactList = JSON.parse(localStorage.getItem('contactList'))
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  width: 450px;
  height: 650px;
  margin: 50px auto;
  background-color: #e2e8f0;
  border-radius: 10px;
  padding: 20px
}

.search-filter__box {
  margin-bottom: 10px;
}

.btn__box {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.btn {
  display: inline-block;
  padding: 12px 18px;
  background-color: #fff;
  border-radius: 10px;
  cursor: pointer;
  font-weight: 500;
  font-size: 18px;
  border: none;
  flex: 1;
}

.btn:hover {
  background-color: #cbd5e1;
}

.btn-small {
  font-size: 12px;
  padding: 8px 10px;
}

.contact__list {
  overflow-y: auto;
  overflow-x: hidden;
  max-height: 400px;
}
.load__box {
  margin-top: auto;
}
</style>
