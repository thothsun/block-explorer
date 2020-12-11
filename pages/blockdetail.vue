<template>

  <div>
    <div style="width: 80%;margin: 0 auto" class="main-content">

      <h1 style="margin-top: 20px">Block Detail</h1>

      <el-divider></el-divider>
      <el-card style="width: 800px;margin: 0 auto">

        <div style="display: flex;justify-content: space-between;align-items: center">
          <div style="font-size: 2rem;">Height {{block.height}}</div>
          <div>{{timestamp2date(block.time)}}</div>
        </div>


        <div class="base-info" style="margin-top: 18px">
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
          <span style="text-align: right">{{block.tx}}</span>
        </div>

      </el-card>

      <el-divider></el-divider>

      <h4>Transactions</h4>

      <el-timeline>
        <el-timeline-item hide-timestamp placement="top" v-for="(tx,index) in txlist">
          <el-card shadow="hover" body-style="border:1px darkgray solid">

            <div class="base-info">
              <span>Txid</span>
              <span>{{tx.txid}}</span>
            </div>
            <div class="base-info">
              <span>Hex</span>
              <div style="overflow: hidden;white-space: normal;word-break: break-all;margin-left: 48px">{{tx.hex}}</div>
            </div>


            <div style="display: flex;justify-content: space-between;margin: 18px 0">
              <div style="width: 45%;display: inline-block">
                <div class="base-info">
                  <span>Size</span>
                  <span>{{tx.size}}</span>
                </div>
                <div class="base-info">
                  <span>Version</span>
                  <span>{{tx.version}}</span>
                </div>
                <div class="base-info">
                  <span>Locktime</span>
                  <span>{{tx.locktime}}</span>
                </div>
              </div>
              <div style="width: 45%;display: inline-block">
                <div class="base-info">
                  <span>Confirmations</span>
                  <span>{{tx.confirmations}}</span>
                </div>
                <div class="base-info">
                  <span>Time</span>
                  <span>{{tx.time}}</span>
                </div>
                <div class="base-info">
                  <span>Blocktime</span>
                  <span>{{tx.blocktime}}</span>
                </div>
              </div>
            </div>

            <div style="display: flex;justify-content: space-around;margin: 18px 0">
              <div style="width: 42%;display: inline-block;border: 1px darkgrey dashed;padding: 0 16px 6px">
                <h5 style="text-align: center">Vin</h5>

                <div v-if="tx.vin[0].coinbase !== undefined" v-for="(item,index) in tx.vin">
                  <div class="base-info">
                    <span>coinbase</span>
                    <span>{{item.coinbase}}</span>
                  </div>
                  <div class="base-info">
                    <span>sequence</span>
                    <span>{{item.sequence}}</span>
                  </div>

                  <el-divider v-if="index!==tx.vin.length-1"></el-divider>
                </div>

                <div v-else v-for="(item,index) in tx.vin">
                  <div class="base-info">
                    <span>txid</span>
                    <div>{{item.txid}}</div>
                  </div>

                  <div class="base-info">
                    <span>vout</span>
                    <span>{{item.vout}}</span>
                  </div>

                  <div class="base-info">
                    <span>sequence</span>
                    <span>{{item.sequence}}</span>
                  </div>

                  <div class="base-info">
                    <span>asm</span>
                    <div>{{item.scriptSig.asm}}</div>
                  </div>

                  <div class="base-info">
                    <span>hex</span>
                    <div>{{item.scriptSig.hex}}</div>
                  </div>

                  <el-divider v-if="index!==tx.vin.length-1"></el-divider>

                </div>

              </div>

              <i class="el-icon-right" style="display: flex;align-items: center;font-size: 36px"></i>

              <div style="width: 42%;display: inline-block;border: 1px darkgrey dashed;padding: 0 16px 6px">
                <h5 style="text-align: center">Vout</h5>


                <div v-for="(item,index) in tx.vout">
                  <div class="base-info">
                    <span>value</span>
                    <span>{{item.value}}</span>
                  </div>
                  <div class="base-info">
                    <span>n</span>
                    <span>{{item.n}}</span>
                  </div>
                  <div class="base-info">
                    <span>reqSigs</span>
                    <span>{{item.scriptPubKey.reqSigs}}</span>
                  </div>
                  <div class="base-info">
                    <span>type</span>
                    <span>{{item.scriptPubKey.type}}</span>
                  </div>
                  <div class="base-info">
                    <span>asm</span>
                    <div>{{item.scriptPubKey.asm}}</div>
                  </div>
                  <div class="base-info">
                    <span>hex</span>
                    <div>{{item.scriptPubKey.hex}}</div>
                  </div>
                  <div class="base-info">
                    <span>addresses</span>
                    <div>{{item.scriptPubKey.addresses}}</div>
                  </div>

                  <el-divider v-if="index!==tx.vout.length-1"></el-divider>
                </div>
              </div>
            </div>

          </el-card>
        </el-timeline-item>
      </el-timeline>

    </div>

    <myfooter></myfooter>
  </div>


</template>

<script>
  import axios from "axios";
  import {username, password} from '~/assets/rpcconfig'
  import myfooter from '~/components/myfooter.vue'

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
            message: 'No data.',
            type: 'info'
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
        this.requestRpc('getrawtransaction', [txid, 1], (result) => {
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
  .base-info {
    display: flex;
    justify-content: space-between;
    color: dimgray;
    margin: 5px 0;
  }

  .base-info span:first-child {
    font-weight: bolder;
  }

  .base-info span:nth-child(2) {
    margin-left: 24px;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .base-info div {
    overflow: hidden;
    white-space: normal;
    word-break: break-all;
    margin-left: 48px;
  }
</style>
