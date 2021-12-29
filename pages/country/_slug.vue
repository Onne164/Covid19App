<template>
<div>
  <v-btn @click="dataLabel='confirmed'">Confirmed</v-btn>
  <v-btn @click="dataLabel='deaths'">Deaths</v-btn>
  <v-row>
    <v-col md4>
      <v-menu
        v-model="WhenStartedDate"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="190px"
      >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        v-model="newEvent.whenStartedDate"
        label="From"
        readonly
        v-bind="attrs"
        v-on="on"
      ></v-text-field>
    </template>
       <v-date-picker v-model="newEvent.whenStartedDate"></v-date-picker>
      </v-menu>
    </v-col>
    <v-col md4>
    </v-col>

    <v-col md4>
    <v-menu
        v-model="WhenEndedDate"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="190px"
    >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        v-model="newEvent.whenEndedDate"
        label="To"
        readonly
        v-bind="attrs"
        v-on="on"
      ></v-text-field>
    </template>
      <v-date-picker v-model="newEvent.whenEndedDate"></v-date-picker>
    </v-menu>
    </v-col>
    <v-col md4>
    </v-col>
</v-row>
  <chart :data="$store.getters[dataLabel]" :labels="$store.getters.labels" :dataLabel="dataLabel"></chart>
</div>
</template>

<script>
import Chart from '~/components/Chart.vue';
export default {
    components: { Chart },
    created() {
      this.$store.dispatch('getCountry', this.$route.params.slug);
    },
    data: vm => ({
      dataLabel: 'confirmed',
      date: new Date().toISOString().substr(0, 10),
      dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
      menu1: false,
      menu2: false,
      WhenStartedDate: null,
      WhenEndedDate: null,
      newEvent: {
      whenStartedDate: null
    }
  }),

    computed: {
       computedDateFormatted () {
       return this.formatDate(this.date)
      },
    },
    watch: {
    date (val) {
      this.dateFormatted = this.formatDate(this.date)
    },
  },

  methods: {
    formatDate (date) {
      if (!date) return null

      const [year, month, day] = date.split('-')
      return `${month}/${day}/${year}`
    },
    parseDate (date) {
      if (!date) return null

      const [month, day, year] = date.split('/')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
     },
   },
 }
</script>

<style>

</style>
