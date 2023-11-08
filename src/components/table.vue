<template>
  <div>
    <!-- Hide By status Bar -->
     
    <div class="hideBar">
      <label class="hideLabel"> Hide: </label>
      <div class="checkbox">
        <!-- All status -->
        <input
          :id="productDataBystatus.status"
          type="checkbox"
          class="styled"
          :value="productDataBystatus.status"
          @click="hideShowALLstatus"
          v-model="hidestatus"
        />
        <label :for="productDataBystatus.status">All statuses</label>

        <!-- Dynamic status -->
        <div v-for="status in productDataBystatus.status" :key="`${status}`">
          <input
            :id="`${status}`"
            type="checkbox"
            class="styled"
            :value="status"
            v-model="hidestatus"
          />
          <label :for="`${status}`">
            {{ status }}
          </label>
        </div>
      </div>
    </div>

   <SearchBar v-model="searchQuery" />

   <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @prevPage="prevPage"
      @nextPage="nextPage"
    />

    <!-- Main Table Design -->
    
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
          <!-- status -->
        
          <tr :class="getStatusClass(status)">
            <td class="width1" :rowspan="calstatusRowspan(data)">
          
              {{ status }}
            </td>
          </tr>

          <template v-for="cores in Object.keys(data)">
            <!-- cores -->
            <tr>
              <td class="width1" :rowspan="Object.keys(data[cores]).length + 1">
                {{ cores }}
              </td>
            </tr>

            <tr v-for="(v, k) in data[cores]">
              <!-- product -->
               <td :class="getStatusClass(status)">{{ v.Product }}</td>

              <!-- Lithography -->
              <td>{{ v.Lithography }}</td>

              <!-- Threads -->
              <td>
                <div class="innerCells">
                  <input :value="v.Threads" :disabled="true" type="text" />
                </div>
              </td>

              <!-- Base Freq -->
              <td>
                <div class="innerCells">
                  <input :value="v.Base_Freq" :disabled="true" type="text" />
                </div>
              </td>

              <!-- Max Turbo Freq -->
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
    <!-- End of Table Design -->
  </div>
</template>


<script>
import data from "../assets/data.json";
import Pagination from "../components/Pagination.vue"
import SearchBar from "../components/SearchBar.vue"

export default {
components: {
    Pagination,
    SearchBar,
  },
  data: function () {
    return {
      hidestatus: [],
      allCheckBox: [],
      UIData: [],
      wwInfo: {},
      allCheck: false,
      currentPage: 1, // Current page
      itemsPerPage: 100, // Number of items per page
      searchQuery: '',
    };
  },

  mounted() {
    this.UIData = data;
    this.wwInfo = this.getWWFromDate();
    
  },
  computed: {
    filteredData() {
    return this.UIData.filter((element) => {
      return element.Product.toLowerCase().includes(this.searchQuery.toLowerCase());
    });
  },
    wwData() {
      return `${this.wwInfo.year}WW${this.wwInfo.workweek}.${this.wwInfo.numofday}`;
    },

    productDataBystatus() {
    const start = (this.currentPage - 1) * this.itemsPerPage;
    //console.log("Start", start);
    const end = start + this.itemsPerPage;
    //console.log("End", end);
      let tmp = {};
      let data = this.filteredData.slice(start, end);
      let statusSet = new Set();

      data.forEach((element) => {
        let status = element.Status;
        let cores = element.Cores;

        // push status to set
        statusSet.add(status);

        if (this.hidestatus.includes(status)) return; // Hide by status
        if (!tmp[status]) tmp[status] = {};
        if (!tmp[status][cores]) tmp[status][cores] = [];

        tmp[status][cores].push(element);
      });

      // sort status in order
      const strings = new Set(statusSet);
      const sortedStringsArray = [...strings].sort();
      statusSet = new Set(sortedStringsArray);

      return {
        status: [...statusSet],
        data: tmp,
      };
    },
    

    totalPages() {
    //console.log("Total Pages will be", Math.ceil(this.UIData.length / this.itemsPerPage));
      //return Math.ceil(this.UIData.length / this.itemsPerPage);
        return Math.ceil(this.filteredData.length / this.itemsPerPage);

    },

     
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
          return ''; // No special class for other statuses
      }
    },

  
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },

    calstatusRowspan(data) {
      let sum = Object.keys(data).length + 1;
      for (const cores in data) {
        sum += Object.keys(data[cores]).length;
      }
      return sum;
    },
    getWWFromDate(date = null) {
      let currentDate = date || new Date();
      let startDate = new Date(currentDate.getFullYear(), 0, 1);
      let days = Math.floor((currentDate - startDate) / (24 * 60 * 60 * 1000));

      return {
        year: currentDate.getFullYear(),
        workweek: Math.ceil(days / 7),
        numofday: currentDate.getDay(),
      };
    },
    hideShowALLstatus() {
      if (!document.querySelector(".styled").checked) {
        this.hidestatus = [];
        this.allCheckBox = [];
      }

      if (document.querySelector(".styled").checked) {
        this.hidestatus = this.productDataBystatus.status;
        this.allCheckBox = this.productDataBystatus.status;
      }

      this.allCheck = !this.allCheck;

      if (this.allCheck) {
      } else {
        this.hidestatus = [];
        this.allCheckBox = [];
      }
    },
    
  },
  
};

</script>


