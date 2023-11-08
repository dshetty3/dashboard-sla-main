<template>
  <div class="hideBar">
    <label class="hideLabel"> Hide: </label>
    <div class="checkbox">
      <!-- All statuses -->
      <input
        :id="allStatusId"
        type="checkbox"
        class="styled"
        value="all"
        @click="toggleAllStatus"
        :checked="selectedStatus.length === productStatus.length"
      />
      <label :for="allStatusId">All statuses</label>

      <!-- Dynamic statuses -->
      <div v-for="status in productStatus" :key="status">
        <input
          :id="status"
          type="checkbox"
          class="styled"
          :value="status"
          :checked="selectedStatus.includes(status)"
          @click="toggleStatus(status)"
        />
        <label :for="status">{{ status }}</label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    productStatus: Array, // List of all product statuses
    selectedStatus: Array, // Selected statuses
  },
  data() {
    return {
      allStatusId: 'allStatusCheckbox',
    };
  },
  methods: {
    toggleAllStatus() {
      if (this.selectedStatus.length === this.productStatus.length) {
        this.selectedStatus = [];
      } else {
        this.selectedStatus = [...this.productStatus];
      }
      this.$emit('update:selectedStatus', this.selectedStatus);
    },
    toggleStatus(status) {
      if (this.selectedStatus.includes(status)) {
        this.selectedStatus = this.selectedStatus.filter((s) => s !== status);
      } else {
        this.selectedStatus.push(status);
      }
      this.$emit('update:selectedStatus', this.selectedStatus);
    },
  },
};
</script>
