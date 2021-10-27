<template>
    <div>
        <el-input
        type="textarea"
        :autosize="{ minRows: 2, maxRows: 4}"
        placeholder="Please input"
        v-model="textarea2">
        </el-input>
    </div>
</template>

<script>
export default {
  data() {
    return {
      textarea2: ''
    }
  }
}
</script>