<template>
  <div class="sort-controls">
    Sort by:
    <span
      class="sort-option"
      v-bind:class="{active: sortBy === 'id'}"
      v-on:click="$emit('sort', 'id')"
    >
      <label>date added</label>
      <span class="sort-icon">{{sortAsc ? "▼" : "▲"}}</span>
    </span> |
    <span
      class="sort-option"
      v-bind:class="{active: sortBy === 'first name'}"
      v-on:click="$emit('sort', 'first name')"
    >
      <label>first name</label>
      <span class="sort-icon">{{sortAsc ? "▼" : "▲"}}</span>
    </span> |
    <span
      class="sort-option"
      v-bind:class="{active: sortBy === 'last name'}"
      v-on:click="$emit('sort', 'last name')"
    >
      <label>last name</label>
      <span class="sort-icon">{{sortAsc ? "▼" : "▲"}}</span>
    </span>
  </div>
</template>

<script>
export default {
  name: "SortControls",
  props: {
    contacts: Array,
    sortBy: String,
    sortAsc: Boolean
  }
};
</script>

<style scoped>
.sort-controls {
  color: #555;
  font-size: 12px;
  text-transform: uppercase;
}

.sort-option {
  color: #777;
  margin-left: 12px;
}

.sort-option label {
  margin-right: 3px;
  cursor: pointer;
  transition: color 0.25s;
}
.sort-option label:hover {
  color: #555;
  border-bottom: 1px solid;
}

.sort-option.active label,
.sort-option.active .sort-icon {
  color: #00cea4;
}

.sort-icon {
  visibility: hidden;
  transition: color 0.25s;
}

.active .sort-icon {
  visibility: visible;
}
</style>


