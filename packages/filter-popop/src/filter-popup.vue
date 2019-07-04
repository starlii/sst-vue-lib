<template>
  <el-popover
    v-bind="$attrs"
    v-on="$listeners"
  >
    <slot></slot>
    <el-button slot="reference" class="select__drop">
      {{label}}:
      <span v-if="common && !format">{{common}}</span>
      <span v-else-if="common && format && format(common)">{{format(common)}}</span>
      <span v-else class="info">Select</span>
      <i class="el-icon-caret-bottom right"></i>
    </el-button>
  </el-popover>
</template>

<script>
  export default {
    name: "filter-popup",
    inheritAttrs: false,
    props: {
      label: {
        type: String,
        default: 'Label'
      },
      common: {
        default: null
      },
      format: {
        type: Function
      }
    }
  }
</script>

<style lang="scss" scoped>
  .select__drop {
    padding: 5px 5px;
    text-align: left;
    border: 1px solid #fff;
    border-radius: 15px;
    color: #606266;
  }

  .iconfont {
    margin-left: 5px;
  }

</style>
