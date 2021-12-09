<template>
  <div class="component-select">

    <select
      class="component-select__select"
      v-model="selectValue"
      @change="$emit('changeSelectValue', selectValue)"
    >

      <option
        class="component-select__option"
        value=""
        disabled
        selected
      >
        Регіон
      </option>

      <option
        class="component-select__option"
        v-for="(key, index) of info"
        :key="index"
        :value="key.regionCode"
      >
        {{ key.regionName }}
      </option>

    </select>
  </div>
</template>

<script>
export default {
  name: "ComponentSelect",
  data() {
    return {
      selectValue: ''
    }
  },
  props: {
    info: {
      type: Array
    }
  }
}
</script>

<style lang="scss" scoped>
.component-select {

  &__select {
    cursor: pointer;
    border: 1px solid #AFAFAF;
    box-sizing: border-box;
    border-radius: 5px;
    height: 50px;
    width: 100%;
    padding-left: 15px;
    font-weight: 500;
    font-size: 16px;
    line-height: 100%;
    display: flex;
    align-items: center;
    text-transform: capitalize;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    overflow: hidden;
    background-color: #FDFDFD;
    background-repeat: no-repeat;
    background-size: 10px 5px;
    background-position: 160px center;
  }

  &__option {
    width: 100%;
  }
}
</style>
