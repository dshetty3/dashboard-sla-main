<template>
  <div class="hideBar">
    <label class="hideLabel">Hide:</label>
    <div class="checkbox">
      <!-- All status -->
      <input
        :id="'allStatus'"
        type="checkbox"
        class="styled"
        :value="productDataBystatus.status"
        @click="toggleAllStatus"
        v-model="localHidestatus"
      />
      <label for="allStatus">All statuses</label>

      <!-- Dynamic status -->
      <div v-for="status in productDataBystatus.status" :key="status">
  <input
    :id="status"
    type="checkbox"
    class="styled"
    :value="status"
    :checked="localHidestatus.includes(status)"
    @change="handleStatusCheckbox(status)"
  />
  <label :for="status">{{ status }}</label>
</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    hidestatus: {
      type: Array,
      default: () => [] // Ensure that hidestatus has a default value of an empty array
    },
    productDataBystatus: Object,
  },
  data() {
    return {
      localHidestatus: this.hidestatus.slice(), // Create a local copy of hidestatus
    };
  },
  methods: {
  toggleAllStatus() {
    if (!this.allCheck) {
      this.localHidestatus = [];
      this.allCheck = true;
    } else {
      this.localHidestatus = this.productDataBystatus.status.slice();
      this.allCheck = false;
    }
    this.$emit('update:hidestatus', this.localHidestatus);
  },
  // Handle individual status checkboxes
  handleStatusCheckbox(status) {
    if (this.localHidestatus.includes(status)) {
      this.localHidestatus = this.localHidestatus.filter(item => item !== status);
    } else {
      this.localHidestatus.push(status);
    }
    this.allCheck = false; // Uncheck the "All statuses" checkbox
    this.$emit('update:hidestatus', this.localHidestatus);
  }
}
};
</script>
