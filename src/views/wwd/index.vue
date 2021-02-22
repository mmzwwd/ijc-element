<template>
  <div class="wwd">
    <el-button type="primary" @click="routw">主要按钮</el-button>
  </div>
</template>

<script>
export default {
  name: 'Wwd',
  components: {},
  data() {
    return {}
  },
  created() {},
  mounted() {},
  methods: {
    routw() {
      this.$router.push({ path: '/dashboard' })
    }
  }
}
</script>
<style  scoped>
</style>
