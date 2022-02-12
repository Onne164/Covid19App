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

    data() {
      return {
        dataLabel: 'confirmed',
        endDate: null,
        startDate: null,
      };
    },

    computed: {
      filteredPeriod() {
      if (this.startDate != null & this.endDate != null ) {
        // var startDate = new Date(this.startDate).toLocaleDateString();
        // var endDate = new Date(this.endDate).toLocaleDateString();

        // this.dataLabel = 'From: ' + startDate + ' To: ' + endDate;
        }
      }
    },
    watch: {
      // date (val) {
      //   this.dateFormatted = this.formatDate(this.date)
      // },
    },

      methods: {
         resetFields: function() {
          this.startDate = '';
          this.endDate = '';
          // this.dataLabel = null;
        }
      }
   }
</script>

<style>

</style>
