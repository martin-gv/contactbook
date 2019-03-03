<template>
  <div>
    <i v-if="!value" class="fa fa-search"></i>
    <input
      type="text"
      class="search"
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  </div>
</template>

<script>
export default {
  name: "Search",
  props: {
    value: String
  }
};
</script>

<style scoped>
.search {
  font-size: 16px;
  font-weight: 300;
  border-bottom: 1px solid #d6d6d6;
}

.fa-search {
  color: rgb(125, 125, 125);
  font-size: 19px;
  position: absolute;
}
</style>
