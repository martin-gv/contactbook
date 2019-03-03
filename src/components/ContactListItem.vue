<template>
  <div v-bind:class="{archived: contact.archived, editing: isEditing}" class="item">
    <transition name="toggle-edit" mode="out-in">
      <div class="main-section" v-if="isEditing" key="editing">
        <edit-contact v-bind:contact="contact" v-on:save="save" v-on:cancel="isEditing = false"/>
      </div>
      <div class="main-section" v-else key="not-editing">
        <div class="contact-info">
          <div>
            <span class="name">{{contact.firstName}} {{contact.lastName}}</span>
          </div>
          <div class="email-phone-section">
            <span class="email">{{contact.email}}</span>
            <span class="phone">{{formattedPhone}}</span>
          </div>
        </div>
        <div class="button-section">
          <div>
            <button class="delete" v-on:click="$emit('remove', contact.id)">Delete</button>
          </div>
          <div>
            <button
              class="archive"
              v-on:click="archive"
            >{{contact.archived ? "Unarchive" : "Archive" }}</button>
            <button class="edit" v-if="!contact.archived" v-on:click="edit">Edit</button>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import EditContact from "./EditContact.vue";

export default {
  name: "ContactListItem",
  components: { EditContact },
  props: { contact: Object },
  data: function() {
    return { isEditing: false };
  },
  methods: {
    archive: function() {
      this.$emit("archive", this.contact.id);
    },
    edit: function() {
      if (this.contact.archived) return;
      this.isEditing = !this.isEditing;
      this.$emit("edit", this.contact.id);
    },
    save: function(data) {
      this.$emit("save", data);
      this.isEditing = false;
    }
  },
  computed: {
    formattedPhone: function() {
      const p = this.contact.phone;
      if (p) {
        const area = p.slice(0, 3);
        const num1 = p.slice(3, 6);
        const num2 = p.slice(6, 10);
        return `(${area}) ${num1}-${num2}`;
      }
      return "";
    }
  }
};
</script>

<style scoped>
.item {
  background-color: #fff;
  text-align: left;
  padding: 20px;
  border-bottom: 1px solid #eaeaea;
  margin: auto;
  max-width: 400px;
  flex-basis: auto;
  transition: all 0.25s ease;
}

.item.first {
  border-top-left-radius: 6px;
  border-top-right-radius: 6px;
}

.item.last {
  border-bottom-left-radius: 6px;
  border-bottom-right-radius: 6px;
  border-bottom: 0;
}

.main-section {
  padding: 10px 10px 0 10px;
  transition: opacity 0.25s ease;
}

.contact-info {
  font-weight: 300;
}

.name {
  color: #555;
  font-size: 20px;
  font-weight: 400;
  text-transform: uppercase;
  display: block;
  margin-bottom: 5px;
}

.email-phone-section {
  display: flex;
  justify-content: space-between;
}

.email {
  color: #838383;
}

.phone {
  color: #838383;
}

.button-section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 20px;
  height: 40px;
}

button {
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 6px 12px;
}

.delete {
  color: #999;
  border: 1px solid #ddd;
}
.delete:hover {
  color: #cd2121;
  border-color: #cd2121;
}

.edit {
  color: #18d1ac;
  border: 1px solid #ddd;
  padding-right: 16px;
  padding-left: 16px;
}
.edit:hover {
  border-color: #18d1ac;
  background-color: #18d1ac;
  color: #fff;
}

.archive {
  color: #999;
  border: 1px solid #ddd;
  margin-right: 5px;
}
.archive:hover {
  color: #555;
  border-color: #aaa;
}

/* Animations */
.toggle-edit-enter-active,
.toggle-edit-leave-active {
  transition: all 50ms;
}
.toggle-edit-enter,
.toggle-edit-leave-to {
  opacity: 0;
}
</style>
