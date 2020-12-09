<template>
  <div style="width: 80%;margin: 0 auto">

    <h1 style="margin-top: 20px">Block Detail</h1>

    <el-divider></el-divider>
    <el-card style="width: 800px;margin: 0 auto">

      <div style="display: flex;justify-content: space-between;align-items: center">
        <div style="font-size: 2rem;">{{block.height}}</div>
        <div>{{timestamp2date(block.time)}}</div>
      </div>


      <div class="base-info">
        <span>Hash</span>
        <span>{{block.hash}}</span>
      </div>

      <div style="display: flex;justify-content: space-between;margin: 18px 0">
        <div style="width: 45%;display: inline-block">
          <div class="base-info">
            <span>Confirmations</span>
            <span>{{block.confirmations}}</span>
          </div>
          <div class="base-info">
            <span>Nonce</span>
            <span>{{block.nonce}}</span>
          </div>
          <div class="base-info">
            <span>Difficulty</span>
            <span>{{block.difficulty}}</span>
          </div>
        </div>
        <div style="width: 45%;display: inline-block">
          <div class="base-info">
            <span>Size</span>
            <span>{{block.size}} bytes</span>
          </div>
          <div class="base-info">
            <span>Bits</span>
            <span>0x{{block.bits}}</span>
          </div>

        </div>
      </div>

      <div class="base-info">
        <span>Merkleroot</span>
        <span>{{block.merkleroot}}</span>
      </div>
      <div class="base-info">
        <span>Previousblockhash</span>
        <span>{{block.previousblockhash}}</span>
      </div>
      <div class="base-info">
        <span>Nextblockhash</span>
        <span>{{block.nextblockhash}}</span>
      </div>
      <div class="base-info">
        <span>Tx</span>
        <span>{{block.tx}}</span>
      </div>

    </el-card>

    <el-divider></el-divider>

    <div v-for="tx in txlist">
      {{tx}}
    </div>

  </div>

</template>

<script>
  import axios from "axios";
  import {username, password} from '~/assets/rpcconfig'

  export default {
    name: "blockdetail",
    data() {
      return {
        block: {},
        txlist: []
      }
    },
    methods: {
      requestRpc(method, params, callback) {
        axios.post('api', {
          method, params
        }, {
          auth: {
            username: username,
            password: password
          }
        }).then((response) => {
          console.log(method + ': ' + response.data.result);
          callback(response.data.result);
        }).catch((error) => {
          console.log(error);
          this.$message({
            duration: 0,
            showClose: true,
            message: 'Please check input hash.',
            type: 'error'
          });
        })
      },

      getBlockDetail(hash) {
        this.requestRpc('getblock', [hash], (result) => {
          this.block = result;

          for (let i = 0; i < result.tx.length; i++) {
            console.log(result.tx[i]);
            this.getTransactionsDetail(result.tx[i]);
          }
        });
      },

      getTransactionsDetail(txid) {
        console.log(txid);
        this.requestRpc('getrawtransaction', [txid,1], (result) => {
          this.txlist.push(result);
        })
      },

      timestamp2date(timestamp) {
        let date = new Date(timestamp * 1000);
        return date.getFullYear() + '-'
          + (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
          + (date.getDate() < 10 ? '0' + date.getDate() : date.getDate()) + ' '
          + (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':'
          + (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
          + (date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds());
      },
    },
    mounted() {
      let hash = this.$route.params.hash;
      console.log(hash);
      this.getBlockDetail(hash);
    }
  }
</script>

<style scoped>

</style>
