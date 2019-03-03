<template>
  <div class="modal-overlay" v-on:click.self="$emit('close')">
    <div class="container">
      <h3>New Contact</h3>
      <div class="row">
        <input
          type="text"
          placeholder="First Name"
          v-model.trim="contact.firstName"
          v-on:keyup.enter="newContact"
          ref="firstName"
        >
        <input
          type="text"
          placeholder="Last Name"
          v-model.trim="contact.lastName"
          v-on:keyup.enter="newContact"
        >
      </div>
      <div class="row">
        <input
          type="email"
          placeholder="Email"
          v-model.trim="contact.email"
          v-on:keyup.enter="newContact"
        >
        <input
          type="text"
          placeholder="Phone"
          maxlength="10"
          v-model.trim="contact.phone"
          v-on:keyup.enter="newContact"
        >
      </div>
      <div class="button-group">
        <button class="cancel" v-on:click="$emit('close')">Cancel</button>
        <button class="add-contact" v-on:click="newContact">Add Contact</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NewContact",
  data: function() {
    return {
      // Properties invoked for clarity, and to prevent error on
      // phone.slice in newContact() method in App.vue
      contact: { firstName: "", lastName: "", email: "", phone: "" }
    };
  },
  methods: {
    newContact: function() {
      // Don't add if all fields blank.
      const keys = Object.keys(this.contact);
      if (!keys.find(el => this.contact[el])) return;
      // Add new contact.
      this.$emit("new-contact", this.contact);
      this.$emit("close");
    }
  },
  mounted: function() {
    this.$refs.firstName.focus();
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.25);
  padding-top: 60px;
  transition: opacity 0.3s ease;
}

.container {
  background-color: #fff;
  max-height: 170px;
  max-width: 300px;
  padding: 20px 30px;
  margin: 10px auto 0;
  border: 1px solid rgb(200, 200, 200);
  border-radius: 3px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

h3 {
  margin: 0 0 20px;
  color: rgb(100, 100, 100);
  font-weight: 400;
  text-transform: uppercase;
}

.row {
  display: flex;
  justify-content: space-between;
}

input {
  font-size: 16px;
  font-weight: 300;
  border-bottom: 1px solid #d6d6d6;
  width: 140px;
  margin-bottom: 25px;
}

input::placeholder {
  font-size: 13px;
  color: rgb(200, 200, 200);
}

.button-group {
  display: flex;
  justify-content: flex-end;
}

button {
  color: #999;
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 6px 12px;
  border: 1px solid #ddd;
}

button.add-contact {
  color: #fff;
  background-color: #18d1ac;
  border: 0;
}
button.add-contact:hover {
  background-color: #14e9be;
}

.cancel {
  margin-right: 10px;
}
.cancel:hover {
  color: #555;
  border-color: #aaa;
}
</style>
