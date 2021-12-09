<template>
  <div id="app" class="app">
    <div class="container">

      <h1 class="app__title">
        Топ 100 транзакцій по регіону
      </h1>

      <graphics
        class="graphics"
        :dataArray="dataArray"
      />

      <component-select
        class="component-select"
        v-if="regions.length"
        :info="regions"
        @changeSelectValue="changeSelectRegionCode"
      />

    </div>
  </div>
</template>

<script>
import Graphics from "./components/Graphics";
import ComponentSelect from "./components/ComponentSelect";

export default {
  name: 'app',
  data() {
    return {
      regions: [],
      selectRegionCode: '',
      regionTransactionsSum: [],
      CORS_SERVER: 'https://api.allorigins.win/get?url=',
      url: 'http://api.spending.gov.ua/api/v2/'
    }
  },
  components: {
    Graphics,
    ComponentSelect
  },
  mounted() {
    this.getRegions()
  },
  computed: {
    dataArray() {
      let arr = this.regionTransactionsSum.concat()
      arr.unshift('Transactions')
      return arr
    }
  },
  methods: {
    getRegions() {
      this.getDataApi('regions', 'regions')
    },
    changeSelectRegionCode(value) {
      this.selectRegionCode = value
      this.getRegionTransactionsSum()
    },
    getRegionTransactionsSum() {
      this.getDataApi('regionTransactionsSum', `api/transactions/top100?region=${this.selectRegionCode}`)
    },
    async getDataApi(val1, val2) {
      try {
        let url = `${this.url}${val2}`
        let res = await fetch(`${this.CORS_SERVER} ${url}`)
        let data = await res.json()

        if (val1 === 'regions') {
          this.regions = await JSON.parse(data.contents)
        } else if (val1 === 'regionTransactionsSum') {
          this.regionTransactionsSum = await JSON.parse(data.contents).map(i => i.amount)
        }

      } catch (error) {
        console.error('Fetch error: ', error);
      }
    }
  }
}
</script>

<style lang="scss">
body {
  font-family: 'Inter', sans-serif;
  overflow-x: hidden;
  font-style: normal;
  font-weight: normal;
  font-size: 24px;
  line-height: 29px;
  color: #111111;
  min-width: 1440px;
}

#app {
  width: 100%;
}

.app {
  padding-top: 30px;

  .container {
    margin: 0 auto;
    background: rgb(64, 44, 254);
    background: linear-gradient(90deg, rgba(64, 44, 254, 1) 0%, rgba(0, 0, 0, 1) 100%);
    padding: 50px 50px 100px;
  }

  &__title {
    text-align: center;
    font-size: 24px;
    line-height: 100%;
    font-weight: 500;
    margin-bottom: 30px;
    color: #fff;
  }

  .graphics {
    margin: 0 auto 30px;
  }
}
</style>
