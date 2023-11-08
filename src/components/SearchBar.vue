<template>
  <div class="search-bar-top-right">
    <input v-model="searchQuery" type="text" placeholder="Search by Product" />
  </div>
</template>

<script>
export default {
  props: {
    modelValue: String, 
  },
  computed: {
    searchQuery: {
      get() {
        return this.modelValue; 
      },
      set(newValue) {
        this.$emit("update:modelValue", newValue); 
      },
    },
  },
};
</script>


