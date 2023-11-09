<template>
  <table>
    <thead>
      <tr>
        <td :colspan="12">Dashboard SLA</td>
      </tr>
      <tr>
        <th colspan="3">{{ wwData }}</th>
        <th colspan="8">Product Info</th>
      </tr>
      <tr>
        <th>Status</th>
        <th>Cores</th>
        <th class="width1">Product</th>
        <th class="width1">Lithography</th>
        <th>Threads</th>
        <th>Base Freq</th>
        <th>Max Turbo Freq</th>
      </tr>
    </thead>

    <tbody>
      <template v-for="(data, status, index) in productDataBystatus.data">
        <tr :class="getStatusClass(status)">
          <td class="width1" :rowspan="calstatusRowspan(data)">{{ status }}</td>
        </tr>

        <template v-for="cores in Object.keys(data)">
          <tr>
            <td class="width1" :rowspan="Object.keys(data[cores]).length + 1">{{ cores }}</td>
          </tr>

          <tr v-for="(v, k) in data[cores]">
            <td :class="getStatusClass(status)">{{ v.Product }}</td>
            <td>{{ v.Lithography }}</td>
            <td>
              <div class="innerCells">
                <input :value="v.Threads" :disabled="true" type="text" />
              </div>
            </td>
            <td>
              <div class="innerCells">
                <input :value="v.Base_Freq" :disabled="true" type="text" />
              </div>
            </td>
            <td>
              <div class="innerCells">
                <input :value="v.Max_Turbo_Freq" type="text" :disabled="true" />
              </div>
            </td>
          </tr>
        </template>
      </template>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    productDataBystatus: Object,
    wwData: String,
  },
  methods: {
    getStatusClass(status) {
      switch (status) {
        case 'Launched':
          return 'status-launched';
        case 'Discontinued':
          return 'status-discontinued';
        case 'Launched (with IPU)':
          return 'status-launchedwithipu';
        case 'Announced':
          return 'status-announced';
        default:
          return ''; 
      }
    },
     calstatusRowspan(data) {
      let sum = Object.keys(data).length + 1;
      for (const cores in data) {
        sum += Object.keys(data[cores]).length;
      }
      return sum;
    },
  },
};
</script>
