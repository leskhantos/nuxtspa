<template>
  <div class="container">
    <p v-if="$fetchState.pending">Fetching data...</p>
    <div v-else>
      <apexchart v-if="series" type="donut" width="600" :options="chartOptions" :series="series"></apexchart>
    </div>

  </div>
</template>

<script>

export default {
 data(){
   return{
     chartOptions: {
       chart: {
         width: 380,
         type: 'pie',
       },
       labels: ['Cases', 'Recovered', 'Active', 'Deaths'],
       responsive: [{
         breakpoint: 480,
         options: {
           chart: {
             width: 200
           },
           legend: {
             position: 'bottom'
           }
         }
       }],
       plotOptions: {
         pie: {
           donut: {
             labels: {
               show: true,
               name: {

               },
               value: {

               }
             }
           }
         }
       }
     },
     data:{

     }
   }
 },
  computed:{
   series: function () {
     return [this.data.cases, this.data.recovered, this.data.active,this.data.deaths]
   }
  },
  async fetch () {
    this.data = await this.$axios.$get('https://corona.lmao.ninja/all')
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
