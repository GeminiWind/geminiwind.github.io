<template>
  <div class="container-displayContext">
    <el-row style="text-align: right; font-size: 4em">
        <el-col :span="24">
            <div class="grid-content bg-purple-dark displayContext">
                {{displayContext || 0}}
            </div>
        </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  props: ["displayContext"]
}
</script>

