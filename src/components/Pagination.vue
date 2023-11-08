<template>
  <div class="centered-pagination">
    <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
    <span>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
    </span>
    <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
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
