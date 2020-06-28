<template>
  <v-flex>
    <v-layout wrap>
    <v-flex xs12>
    <v-btn @click="simulateDynamicData">fake load dynamic data</v-btn>
    </v-flex>
    <v-flex xs12>
    <areaChart :dataSet2="dataSet3"></areaChart>
    </v-flex>
    </v-layout>

    <v-flex xs12>
    <bar-chart :dataSet="dataSet" @hover="showTooltip" @out="tooltip.isActive = false"></bar-chart>
    <v-tooltip top :activator="tooltip.id" v-model="tooltip.isActive">
    <span>Frequency: {{tooltip.text}}</span>
    </v-tooltip>
    </v-flex>
    <v-flex xs12>
    <pieChart :data="dataSet2"></pieChart>
    </v-flex>

  </v-flex>
</template>

<script>
import barChart from '../components/barChart'
import pieChart from '../components/pieChart'
import areaChart from '../components/areaChart'
import * as d3 from 'd3'
export default {
  components: {
    barChart,
    pieChart,
    areaChart
  },
  created () {
    this.loadData()
    this.loadData2()
  },
  data: () => ({
    dataSet3: null,
    dataToAgregate: [],
    startDynamic: false,
    dataSet: null,
    tooltip: {
      isActive: false,
      id: '',
      text: ''
    },
    dataSet2: [
      { name: 'Tedy', value: 2502 },
      { name: 'Fazrin', value: 2005 },
      { name: 'Laura', value: 1968 },
      { name: 'Chloé', value: 1863 },
      { name: 'Marie', value: 1810 }]
  }),
  methods: {
    loadData () {
      d3.tsv('./static/example-data/barChartData.tsv').then(data => {
        data.map(d => {
          d.frequency = +d.frequency
        })
        this.dataSet = data
      })
    },
    async loadData2 () {
      let parseTime = d3.timeParse('%d-%b-%y')
      let data1 = await d3.tsv('/static/example-data/data.tsv')
      let data2 = await d3.tsv('/static/example-data/data1.tsv')
      data1.map(d => {
        d.date = parseTime(d.date)
        d.close = +d.close
      })
      data2.map(d => {
        d.date = parseTime(d.date)
        d.close = +d.close
      })
      this.dataSet3 = data1
      this.dataToAgregate = data2
    },
    simulateDynamicData () {
      let i = 0
      this.startDynamic = true
      const interval = setInterval(() => {
        i++
        if (!this.startDynamic || i === this.dataToAgregate.length - 1) {
          clearInterval(interval)
        } else {
          this.dataSet3.push(this.dataToAgregate[i])
        }
      }, 100)
    },
    showTooltip (e) {
      this.tooltip = {
        isActive: true,
        id: e.id,
        text: e.text
      }
    }
  }
}
</script>
