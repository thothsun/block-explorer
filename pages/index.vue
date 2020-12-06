<template>
  <div>
    创世区块hash：{{ genesisBlockHash }}
  </div>
</template>

<script>
  import axios from "axios";
  import {username, password} from '~/assets/rpcconfig'

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
        "params": [0],
      }, {
        auth: {
          username: username,
          password: password
        }
      }).then((response) => {
        this.genesisBlockHash = response.data.result
      })
    }
  }
</script>

<style>


</style>
