<template>
  <div class="container">
    <p v-if="$fetchState.pending">Fetching data...</p>
    <div v-else>
      <div class="d-flex flex-column">
        <h2>
          {{(new Date(this.data.updated)).toLocaleTimeString()}}
        </h2>
        <h1>
          {{(new Date(this.data.updated)).toLocaleDateString()}}
        </h1>
        <apexchart v-if="series" type="donut" width="400" :options="chartOptions" :series="series"></apexchart>
        <apexchart v-if="seriesKZ" type="donut" width="400" :options="chartOptionsKZ" :series="seriesKZ"></apexchart>
        <apexchart v-if="seriesRU" type="donut" width="400" :options="chartOptionsRU" :series="seriesRU"></apexchart>
      </div>
    </div>
  </div>
</template>

<script>

export default {
 data(){
   return{
     chartOptions: {
       chart: {
         width: 400,
         type: 'pie',
         animations: {
           enabled: true,
           easing: 'easeinout',
           speed: 800,
           animateGradually: {
             enabled: true,
             delay: 150
           },
           dynamicAnimation: {
             enabled: true,
             speed: 350
           }
         }
       },
       title: {
         text: "В мире",
         margin: 5,
         style: {
           fontSize: '15px',
           fontWeight: 'bold',
         },
       },
       dataLabels: {
         formatter: function (val, opts) {
           return opts.w.config.series[opts.seriesIndex]
         },
       },
       labels: ['Выздоровевших', 'Заболевших', 'Умерших'],
       colors: ['#00FF00', '#FF0000', '#000000'],
       responsive: [{
         breakpoint: 480,
         options: {
           chart: {
             width: '100%'
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

               },
               total: {
                 show: true,
                 showAlways: false,
                 label: 'Всего',
                 fontSize: '15px',
                 fontFamily: 'Helvetica, Arial, sans-serif',
                 fontWeight: 700,
                 color: '#373d3f',
                 formatter: function (w) {
                   return w.globals.seriesTotals.reduce((a, b) => {
                     return a + b
                   }, 0)
                 }
               }
             }
           }
         }
       }
     },
     data:{},
     dataKZ:{},
     dataRU:{},
     chartOptionsKZ: {
       chart: {
         width: 400,
         type: 'pie',
         animations: {
           enabled: true,
           easing: 'easeinout',
           speed: 800,
           animateGradually: {
             enabled: true,
             delay: 150
           },
           dynamicAnimation: {
             enabled: true,
             speed: 350
           }
         }
       },
       title: {
         text: "KZ",
         margin: 5,
         style: {
           fontSize: '15px',
           fontWeight: 'bold',
         },
       },
       dataLabels: {
         formatter: function (val, opts) {
           return opts.w.config.series[opts.seriesIndex]
         },
       },
       labels: ['Выздоровевших', 'Заболевших', 'Умерших'],
       colors: ['#00FF00', '#FF0000', '#000000'],
       responsive: [{
         breakpoint: 480,
         options: {
           chart: {
             width: '100%'
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

               },
               total: {
                 show: true,
                 showAlways: false,
                 label: 'Всего',
                 fontSize: '15px',
                 fontFamily: 'Helvetica, Arial, sans-serif',
                 fontWeight: 700,
                 color: '#373d3f',
                 formatter: function (w) {
                   return w.globals.seriesTotals.reduce((a, b) => {
                     return a + b
                   }, 0)
                 }
               }
             }
           }
         }
       }
     },
     chartOptionsRU: {
       chart: {
         width: 400,
         type: 'pie',
         animations: {
           enabled: true,
           easing: 'easeinout',
           speed: 800,
           animateGradually: {
             enabled: true,
             delay: 150
           },
           dynamicAnimation: {
             enabled: true,
             speed: 350
           }
         }
       },
       title: {
         text: "RUS",
         margin: 5,
         style: {
           fontSize: '15px',
           fontWeight: 'bold',
         },
       },
       dataLabels: {
         formatter: function (val, opts) {
           return opts.w.config.series[opts.seriesIndex]
         },
       },
       labels: ['Выздоровевших', 'Заболевших', 'Умерших'],
       colors: ['#00FF00', '#FF0000', '#000000'],
       responsive: [{
         breakpoint: 480,
         options: {
           chart: {
             width: '100%'
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

               },
               total: {
                 show: true,
                 showAlways: false,
                 label: 'Всего',
                 fontSize: '15px',
                 fontFamily: 'Helvetica, Arial, sans-serif',
                 fontWeight: 700,
                 color: '#373d3f',
                 formatter: function (w) {
                   return w.globals.seriesTotals.reduce((a, b) => {
                     return a + b
                   }, 0)
                 }
               }
             }
           }
         }
       }
     }

   }
 },
  computed:{
   series: function () {
     return [this.data.recovered, this.data.active,this.data.deaths]
   }, seriesKZ: function () {
     return [this.dataKZ.recovered, this.dataKZ.active,this.dataKZ.deaths]
   }, seriesRU: function () {
     return [this.dataRU.recovered, this.dataRU.active,this.dataRU.deaths]
   }
  },
  async fetch () {
    this.data = await this.$axios.$get('https://corona.lmao.ninja/all')
    this.dataKZ = await this.$axios.$get('https://corona.lmao.ninja/countries/KZ')
    this.dataRU = await this.$axios.$get('https://corona.lmao.ninja/countries/RUS')
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
