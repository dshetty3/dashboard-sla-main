<template>
  <div>
    <!-- Hide By status Bar -->
    <StatusFilter v-model="hidestatus" @update:hidestatus="updateHidestatus" :productDataBystatus="productDataBystatus" />


  <!-- Search bar to filter out with Product Criteria -->
   <SearchBar v-model="searchQuery" />

  <!-- Pagination for 100 rows/page -->
   <Pagination
      :currentPage="currentPage"
      :totalPages="totalPages"
      @prevPage="prevPage"
      @nextPage="nextPage"
    />

  <!-- Main Table Design -->
     <DataTable :productDataBystatus="productDataBystatus" :wwData="wwData" />
    <!-- End of Table Design -->
  </div>
</template>


<script>
import data from "../assets/data.json";
import Pagination from "../components/Pagination.vue"
import SearchBar from "../components/SearchBar.vue"
import DataTable from "../components/DataTable.vue";
import StatusFilter from "../components/StatusFilter.vue";


export default {
components: {
    Pagination,
    SearchBar,
    DataTable,
    StatusFilter,
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

   updateHidestatus(updatedHidestatus) {
      this.hidestatus = updatedHidestatus;
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


