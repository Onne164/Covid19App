<template>
<div>
  <v-btn @click="dataLabel='confirmed'">Confirmed</v-btn>
  <v-btn @click="dataLabel='deaths'">Deaths</v-btn>
  <v-btn @click="filteredPeriod">Apply Date Filter</v-btn>
  <v-btn @click="resetFields">Reset</v-btn>
  <v-row >
        <v-col cols="2">
          <v-text-field v-model="startDate"  label="From" single-line>
            <template v-slot:append-outer>
              <date-picker v-model="startDate"/>
            </template>
          </v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field v-model="endDate"  label="To" single-line>
            <template v-slot:append-outer>
              <date-picker v-model="endDate"/>
            </template>
          </v-text-field>
        </v-col>
  </v-row>
  <chart :data="$store.getters[dataLabel]" :labels="$store.getters.labels" :dataLabel="dataLabel" ></chart>
</div>
</template>

<script>
import Chart from '~/components/Chart.vue';
import DatePicker from '~/components/DatePicker.vue';
export default {
    components: { Chart, DatePicker },

    created() {
      this.$store.dispatch('getCountry', this.$route.params.slug);
    },

    data: () => ({
        dataLabel: 'confirmed',
        endDate: null,
        startDate: null,
        chart: null,
        data: [],
        labels: [],
      }),

    mounted() {

    },
    computed: {

    },
    watch: {

    },
      methods: {
         resetFields() {
          this.startDate = '';
          this.endDate = '';
        },
        filteredPeriod(start, end) {
          let startDate = new Date(this.startDate).toLocaleDateString();
          let endDate = new Date(this.endDate).toLocaleDateString();

          if (startDate != null & endDate != null ) {
              let newLabels = [...this.$store.getters.labels];
     
              // get the index number in array
              let indexStartDate = newLabels.indexOf(startDate);
              let indexEndDate = newLabels.indexOf(endDate);
              // console.log(indexStartDate);
              // console.log(indexEndDate);

              // slice the array only showing the selected period
              let selectedPeriod = newLabels.slice(indexStartDate, indexEndDate + 1);
              this.labels = selectedPeriod;


              let newData = [...this.$store.getters[this.dataLabel]];
              let filteredData = newData.slice(indexStartDate, indexEndDate + 1);

              this.data = filteredData;

          }
        }
      }}

</script>

<style>

</style>
