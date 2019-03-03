<template>
  <div id="app">
    <header>
      <h1>Contact Book</h1>
      <div class="toolbar">
        <div class="toolbar-item">
          <search v-model.trim="searchText"/>
        </div>
        <div class="toolbar-item">
          <sort-controls
            v-bind:contacts="activeContacts"
            v-bind:sortBy="sortBy"
            v-bind:sortAsc="sortAsc"
            v-on:sort="sort"
          />
        </div>
        <div class="toolbar-item">
          <button class="new-contact-button" v-on:click="showAddNew = !showAddNew">New Contact</button>
        </div>
      </div>
    </header>
    <transition name="modal">
      <new-contact
        v-if="showAddNew"
        v-on:new-contact="newContact"
        v-on:close="showAddNew = !showAddNew"
      />
    </transition>
    <contact-list
      v-bind:contacts="activeContacts"
      v-on:remove="remove"
      v-on:archive="archive"
      v-on:save="save"
      ref="activeContactList"
    />
    <div class="message" v-if="searchText && !activeContacts.length">No contacts found</div>
    <div class="message" v-if="!searchText && !activeContacts.length">No contacts</div>
    <button
      v-if="archivedContacts.length"
      class="archived-contacts"
      v-on:click="showArchive = !showArchive"
    >{{showArchive ? "Hide Archive -" : "Show Archived Contacts +"}}</button>
    <div
      class="message"
      v-if="!archivedContacts.length && activeContacts.length"
    >No archived contacts</div>
    <contact-list
      v-if="showArchive"
      v-bind:contacts="archivedContacts"
      v-on:remove="remove"
      v-on:archive="archive"
      v-on:save="save"
    />
  </div>
</template>

<script>
import NewContact from "./components/NewContact.vue";
import SortControls from "./components/SortControls.vue";
import Search from "./components/Search.vue";
import ContactList from "./components/ContactList.vue";

import contacts from "./assets/sampleData.js";

let nextId = 7;

export default {
  name: "app",
  components: {
    NewContact,
    SortControls,
    Search,
    ContactList
  },
  data: function() {
    return {
      contacts,
      showAddNew: false,
      showArchive: false,
      sortBy: "id",
      sortAsc: false,
      searchText: ""
    };
  },
  methods: {
    newContact: function(data) {
      this.contacts.push({
        id: nextId++,
        firstName: data.firstName,
        lastName: data.lastName,
        email: data.email,
        // Enforces 10 character limit set in HTML input element.
        phone: data.phone.slice(0, 10),
        archived: false
      });
    },
    remove: function(id) {
      this.contacts = this.contacts.filter(el => el.id !== id);
    },
    archive: function(id) {
      const index = this.contacts.findIndex(el => el.id === id);
      const update = {
        ...this.contacts[index],
        archived: !this.contacts[index].archived
      };
      this.contacts.splice(index, 1, update);
    },
    save: function(data) {
      const index = this.contacts.findIndex(el => el.id === data.id);
      const update = { ...data, phone: data.phone.slice(0, 10) };
      this.contacts.splice(index, 1, update);
    },
    sort: function(type) {
      if (this.sortBy === type) {
        this.sortAsc = !this.sortAsc;
      }
      this.sortBy = type;
      this.$refs.activeContactList.closeEdit();
    }
  },
  computed: {
    activeContacts: function() {
      let contacts = this.contacts.filter(el => !el.archived);
      const term = this.searchText.toLowerCase();

      if (term) {
        contacts = contacts.filter(el => {
          const name = (el.firstName + " " + el.lastName).toLowerCase();
          if (name.includes(term)) return true;
          if (el.email.includes(term)) return true;
          if (el.phone.includes(term)) return true;

          // Alternate search style. Doesn't match substrings.
          // if (el.firstName.toLowerCase().startsWith(term)) return true;
          // if (el.lastName.toLowerCase().startsWith(term)) return true;
        });
      }

      return contacts.sort((a, b) => {
        if (this.sortBy === "id") {
          return this.sortAsc ? a.id - b.id : b.id - a.id;
        } else if (this.sortBy === "first name") {
          const nameA = a.firstName.toLowerCase();
          const nameB = b.firstName.toLowerCase();

          if (this.sortAsc) {
            if (nameA < nameB) return -1;
            if (nameA > nameB) return 1;
          } else {
            if (nameA < nameB) return 1;
            if (nameA > nameB) return -1;
          }

          return 0;
        } else if (this.sortBy === "last name") {
          const nameA = a.lastName.toLowerCase();
          const nameB = b.lastName.toLowerCase();

          if (this.sortAsc) {
            if (nameA < nameB) return -1;
            if (nameA > nameB) return 1;
          } else {
            if (nameA < nameB) return 1;
            if (nameA > nameB) return -1;
          }

          return 0;
        }
      });
    },
    archivedContacts: function() {
      return this.contacts.filter(el => el.archived);
    }
  }
};
</script>

<style>
body {
  font-family: "Lato", "Helvetica", sans-serif;
  text-align: center;
  background-color: #efedee;
  margin: 0;
}

header {
  background-color: #fff;
  padding: 20px 30px;
  margin-bottom: 30px;
  box-shadow: 0 10px 20px #e5e5e5;
  width: 100%;
  box-sizing: border-box;
}

h1 {
  color: #414141;
  font-size: 20px;
  font-weight: 400;
  letter-spacing: 3px;
  text-transform: uppercase;
  margin: 0 0 5px;
}

button {
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
  border-radius: 2px;
}

input {
  font-family: "Lato", "Helvetica", sans-serif;
  background-color: rgb(0, 0, 0, 0);
  border: none;
  outline: none;
}

.toolbar {
  display: flex;
  align-items: center;
}

.toolbar-item {
  flex: 1;
}

.toolbar-item:nth-child(2) {
  flex: 2;
}

.toolbar-item > input {
  display: block;
  margin-right: auto;
}

.toolbar-item > button {
  display: block;
  margin-left: auto;
}

@media only screen and (max-width: 850px) {
  .toolbar {
    flex-direction: column;
  }

  .toolbar-item {
    margin: 10px 0;
  }
}

button.new-contact-button {
  color: rgb(245, 245, 245);
  background-color: #333;
  font-size: 11px;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 8px 10px;
}
button.new-contact-button:hover {
  color: #fff;
}

button.archived-contacts,
.message {
  color: rgb(100, 100, 100);
  font-size: 11px;
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-bottom: 30px;
}
button.archived-contacts:hover {
  color: rgb(0, 0, 0);
}

/* Animations */
.modal-enter,
.modal-leave-to {
  opacity: 0;
}

.modal-enter .container,
.modal-leave-to .container {
  transform: scale(1.025);
}
</style>
