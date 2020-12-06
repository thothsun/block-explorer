<template>
  <div>
    创世区块hash：{{ genesisBlockHash }}
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      genesisBlockHash: ''
    }
  },
  methods: {},
  mounted() {
    axios.post('api', {
      "method": "getblockhash",
      "params": [0]
    }).then((response) => {
      this.genesisBlockHash = response.data.result
    })
  }
}
</script>

<style>


</style>
