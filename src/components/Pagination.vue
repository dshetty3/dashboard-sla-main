<template>
  <div class="centered-pagination">
    <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
  
      &nbsp; Page {{ currentPage }} of {{ totalPages }} &nbsp;
    
    <button @click="nextPage" :disabled="currentPage === totalPages">  Next</button>
  </div>
</template>

<script>
export default {
  props: ['currentPage', 'totalPages'],
  methods: {
    prevPage() {
      this.$emit('prevPage');
    },
    nextPage() {
      this.$emit('nextPage');
    },
  },
};
</script>
