<template>
  <div class="contact-list">
    <transition-group name="list" tag="span">
      <contact-list-item
        v-for="contact in contacts"
        v-bind:key="contact.id"
        v-bind:contact="contact"
        v-bind:class="{first: contact === contacts[0], last: contact === contacts[contacts.length - 1]}"
        v-on:remove="remove"
        v-on:archive="archive"
        v-on:save="save"
        v-on:edit="closeEdit"
        ref="contactListItem"
      />
    </transition-group>
  </div>
</template>

<script>
import ContactListItem from "./ContactListItem";

export default {
  name: "ContactList",
  components: {
    ContactListItem
  },
  props: {
    contacts: Array
  },
  methods: {
    remove: function(id) {
      this.$emit("remove", id);
    },
    archive: function(id) {
      this.$emit("archive", id);
    },
    save: function(data) {
      this.$emit("save", data);
    },
    closeEdit: function(id) {
      const filtered = this.$refs.contactListItem.filter(
        el => el.contact.id !== id
      );
      filtered.forEach(el => (el.isEditing = false));
    }
  }
};
</script>

<style scoped>
.contact-list {
  margin-bottom: 20px;
}

/* Animations */
.list-enter-active,
.list-leave-active {
  transition: all 0.25s ease;
}
.list-leave-active {
  position: absolute;
}
.list-enter,
.list-leave-to {
  opacity: 0;
}
.list-move {
  transition: transform 1s ease;
}
</style>

