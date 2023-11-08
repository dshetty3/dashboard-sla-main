<template>
  <div class="hideBar">
    <label class="hideLabel">Hide:</label>
    <div class="checkbox">
      <input
        :id="productDataBystatus.status"
        type="checkbox"
        class="styled"
        :value="productDataBystatus.status"
        @click="toggleAllStatus"
        v-model="hidestatus"
      />
      <label :for="productDataBystatus.status">All statuses</label>

      <div v-for="status in productDataBystatus.status" :key="status">
        <input
          :id="status"
          type="checkbox"
          class="styled"
          :value="status"
          v-model="hidestatus"
        />
        <label :for="status">{{ status }}</label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    productDataBystatus: Object,
    hidestatus: Array,
  },
  methods: {
    toggleAllStatus() {
      if (this.hidestatus.length === this.productDataBystatus.status.length) {
        this.hidestatus = [];
      } else {
        this.hidestatus = [...this.productDataBystatus.status];
      }
    },
  },
};
</script>
