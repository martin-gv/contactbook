<template>
  <div class="edit-contact">
    <h3>Editing Contact</h3>
    <div class="form-section">
      <div class="row">
        <input
          type="text"
          placeholder="First Name"
          v-model.trim="copy.firstName"
          v-on:keyup.enter="save"
          ref="firstName"
        >
        <input
          type="text"
          placeholder="Last Name"
          v-model.trim="copy.lastName"
          v-on:keyup.enter="save"
        >
      </div>
      <div class="row">
        <input type="text" placeholder="Email" v-model.trim="copy.email" v-on:keyup.enter="save">
        <input
          type="text"
          placeholder="Phone"
          maxlength="10"
          v-model.trim="copy.phone"
          v-on:keyup.enter="save"
        >
      </div>
    </div>
    <div class="button-section">
      <button class="cancel" v-on:click="$emit('cancel')">Cancel</button>
      <button class="save" v-on:click="save">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditContact",
  props: { contact: Object },
  data: function() {
    return {
      // Properties invoked for clarity, and to prevent error on
      // phone.slice in save() method in App.vue
      copy: { firstName: "", lastName: "", email: "", phone: "" }
    };
  },
  methods: {
    save: function() {
      this.$emit("save", this.copy);
    }
  },
  created: function() {
    this.copy = { ...this.contact };
  },
  mounted: function() {
    this.$refs.firstName.focus();
  }
};
</script>

<style scoped>
h3 {
  font-weight: 400;
  color: rgb(100, 100, 100);
  color: #00a281;
  margin-top: 0;
}

.row {
  display: flex;
  justify-content: space-between;
}

.row:last-of-type {
  margin-bottom: 5px;
}

input {
  font-size: 16px;
  font-weight: 300;
  border-bottom: 1px solid #d6d6d6;
  flex-basis: 150px;
  margin-bottom: 15px;
}

input::placeholder {
  font-size: 13px;
  color: rgb(200, 200, 200);
}

.button-section {
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

button.save {
  color: #fff;
  background-color: #18d1ac;
  border: 0;
}
button.save:hover {
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


