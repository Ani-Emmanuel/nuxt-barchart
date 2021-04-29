<template>
  <div class="container">
    <bar-chart
      v-if="condition"
      :data="barChartData"
      :options="barChartOptions"
      :height="200"
    />
    <div
      v-else
      style="
         {
          position: relative;
          text-align: center;
          justify-self: center;
          align-self: center;
        }
      "
    >
      <h1>Load....</h1>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BarChart from '~/components/barChart'

const chartColors = {
  red: 'rgb(255, 99, 132)',
  orange: 'rgb(255, 159, 64)',
  yellow: 'rgb(255, 205, 86)',
  green: 'rgb(75, 192, 192)',
  blue: 'rgb(54, 162, 235)',
  purple: 'rgb(153, 102, 255)',
  grey: 'rgb(201, 203, 207)',
}

const payload = []

export default {
  components: {
    BarChart,
  },
  data() {
    return {
      barChartData: {
        labels: [
          '1',
          '2',
          '3',
          '4',
          '5',
          '6',
          '7',
          '8',
          '9',
          '10',
          '11',
          '12',
          '13',
          '14',
          '15',
          '16',
          '17',
          '18',
          '19',
          '20',
        ],
        datasets: [
          {
            label: 'Market_Cap_Usd',
            backgroundColor: [
              chartColors.red,
              chartColors.orange,
              chartColors.yellow,
              chartColors.green,
              chartColors.blue,
              chartColors.purple,
              chartColors.gray,
              chartColors.red,
              chartColors.green,
              chartColors.purple,
              chartColors.blue,
              chartColors.purple,
              chartColors.red,
              chartColors.gray,
              chartColors.purple,
              chartColors.yellow,
              chartColors.green,
              chartColors.blue,
              chartColors.red,
              chartColors.yellow,
            ],
            data: payload,
          },
        ],
      },
      barChartOptions: {
        responsive: true,
        legend: {
          display: false,
        },
        title: {
          display: true,
          text: 'Market Cap Usd',
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true,
              },
            },
          ],
        },
      },
      condition: false,
    }
  },

  // makes ajax call to get the data
  async fetch() {
    await axios
      .get('https://api.coinlore.net/api/tickers/')
      .then((res) => {
        const doc = res.data.data
        const temp = doc.sort((a, b) => a.rank - b.rank)
        temp.forEach((element) => {
          payload.push(Number(element.market_cap_usd))
        })

        this.condition = true
      })
      .catch((err) => {
        throw err
      })
  },
}
</script>
