<template>
  <el-col :span="span">
    <el-button plain @click="press(btnValue)">{{btnLabel || btnValue}}</el-button>
  </el-col>
</template>

<script>
export default {
  props: ["btnValue", "span", "btnLabel", "color"],
  methods: {
    press(btnValue) {
       this.$emit('press', this.btnValue)
    }
  }
}
</script>
