<template>
  <div class="graphics">
    <vue-c3 class="vue-c3"
            :handler="handler" load></vue-c3>
  </div>
</template>

<script>
import Vue from 'vue'
import VueC3 from 'vue-c3'

export default {
  name: "Graphics",
  data() {
    return {
      handler: new Vue(),
      options: {
        data: {
          columns: [],
          colors: {
            Transactions: 'rgb(64,44,254)',
          },
          axes: {
            Transactions: 'y2'
          },
          types: {
            Transactions: 'bar'
          },
        },
        axis: {
          y: {
            label: {
              text: 'Suma',
              position: 'outer-middle'
            },
          }
        }
      },
    }
  },
  components: {
    VueC3
  },
  props: {
    dataArray: {
      type: Array
    }
  },
  watch: {
    dataArray: {
      handler() {
        this.options.data.columns = [this.dataArray]
        this.handler.$emit('destroy')
        this.changeGraphics()
      },
      deep: true
    }
  },
  created() {
    this.options.data.columns = [this.dataArray]
  },
  mounted() {
    this.changeGraphics()
  },
  methods: {
    changeGraphics() {
      this.handler.$emit('init', this.options)
    }
  }
}
</script>

<style lang="scss" scoped>
.graphics {
  width: 100%;
  background-color: white;
  padding: 20px 30px;
}
</style>
