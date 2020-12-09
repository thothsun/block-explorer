<template>
  <div>{{hash}}</div>
</template>

<script>
  export default {
    name: "blockdetail",
    data() {
      return {
        hash: ''
      }
    },
    mounted() {
      let hash = this.$route.params.hash;
      this.hash = hash;
    }
  }
</script>

<style scoped>

</style>
