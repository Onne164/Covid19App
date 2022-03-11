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
  <chart :data="$store.getters[dataLabel]" :labels="$store.getters.labels" :dataLabel="dataLabel" :key="chartKey" ></chart>
</div>
</template>

<script>
import Chart from '~/components/Chart.vue';
import DatePicker from '~/components/DatePicker.vue';
export default {
    components: { Chart, DatePicker },

    created() {
      // console.log(this)
      let passData = {
          slug: this.$route.params.slug,
          startDate: this.startDate,
          endDate: this.endDate
      }
      this.$store.dispatch('getCountry', passData);
    },

    data: () => ({
        dataLabel: 'confirmed',
        endDate: null,
        startDate: null,
        chart: null,
        data: [],
        labels: [],
        chartKey: 0
      }),

    mounted() {

    },
    computed: {

    },
    watch: {

    },
      methods: {
         resetFields() {
          this.startDate = null;
          this.endDate = null;
          let passData = {
            slug: this.$route.params.slug,
            startDate: this.startDate,
            endDate: this.endDate
          }
          this.$store.dispatch('getCountry', passData);
        },
        async filteredPeriod(start, end) {
          if (this.startDate) {
            let passData = {
            slug: this.$route.params.slug,
            startDate: this.startDate,
            endDate: this.endDate
          }
          this.$store.dispatch('getCountry', passData);
          }
              this.chartKey++
        }
      }}

</script>

<style>

</style>
