<template>
  <div style="width: 80%;margin: 0 auto">

    <h1 style="margin-top: 20px">Block Explorer</h1>

    <el-divider></el-divider>

    <div style="display: flex;justify-content: space-between">

      <div style="width: 30%;display: inline-block">
        <h3>Network State</h3>

        <div>
          <div class="base-info">
            <span>block counts</span>
            <span>{{blockCount}}</span>
          </div>

          <div class="base-info">
            <span>difficulty</span>
            <span>{{currentDifficulty}}</span>
          </div>

        </div>
      </div>

      <div style="width: 60%;display: inline-block">
        <h3>Recent Blocks</h3>

        <el-table
          :data="blockList"
          style="width: 80%"
          border>
          <el-table-column
            label="height"
            prop="height"
            min-width="20">
          </el-table-column>

          <el-table-column
            label="hash"
            prop="hash"
            min-width="80">
          </el-table-column>

        </el-table>
      </div>

    </div>

    <el-divider></el-divider>

    <div style="display: flex;justify-content: space-between">

      <div style="width: 48%;display: inline-block">
        <h3>Block Detail</h3>
        <div>
          <div class="base-info">
            <span>height</span>
            <span>{{height}}</span>
          </div>
          <div class="base-info">
            <span>confirmations</span>
            <span>{{confirmations}}</span>
          </div>
          <div class="base-info">
            <span>size</span>
            <span>{{size}}</span>
          </div>
          <div class="base-info">
            <span>time</span>
            <span>{{time}}</span>
          </div>
          <div class="base-info">
            <span>nonce</span>
            <span>{{nonce}}</span>
          </div>
          <div class="base-info">
            <span>bits</span>
            <span>{{bits}}</span>
          </div>
          <div class="base-info">
            <span>difficulty</span>
            <span>{{difficulty}}</span>
          </div>
          <div class="base-info">
            <span>merkleroot</span>
            <span>{{merkleroot}}</span>
          </div>
          <div class="base-info">
            <span>hash</span>
            <span>{{hash}}</span>
          </div>
          <div class="base-info">
            <span>previousblockhash</span>
            <span>{{previousblockhash}}</span>
          </div>
          <div class="base-info">
            <span>nextblockhash</span>
            <span>{{nextblockhash}}</span>
          </div>

          <div class="base-info">
            <span>transactions</span>
            <span style="text-align: right">
            <div v-for="item in transactions">{{item}}</div>
          </span>
          </div>


        </div>
      </div>

      <div style="width: 48%;display: inline-block">
        <h3>Transactions Detail</h3>
        <div>
          <div class="base-info">
            <span>fee</span>
            <span>{{fee}}</span>
          </div>

          <div class="base-info">
            <span>blockindex</span>
            <span>{{blockindex}}</span>
          </div>

          <div class="base-info">
            <span>blocktime</span>
            <span>{{blocktime}}</span>
          </div>

          <div class="base-info">
            <span>txid</span>
            <span>{{txid}}</span>
          </div>

          <div class="base-info">
            <span>timereceived</span>
            <span>{{timereceived}}</span>
          </div>

          <div class="base-info">
            <span>comment</span>
            <span>{{comment}}</span>
          </div>

          <div class="base-info">
            <span>hex</span>
            <div style="width:600px;display:block;word-break: break-all;word-wrap: break-word;">{{hex}}</div>
          </div>


        </div>
      </div>

    </div>


    <el-divider></el-divider>

  </div>
</template>

<script>
  import axios from "axios";
  import {username, password} from '~/assets/rpcconfig'

  export default {
    data() {
      return {
        //part1
        blockCount: '-',
        currentDifficulty: '-',
        //part2
        blockList: [],
        //part3
        height: '-',
        confirmations: '-',
        size: '-',
        time: '-',
        nonce: '-',
        bits: '-',
        difficulty: '-',
        merkleroot: '-',
        hash: '-',
        previousblockhash: '-',
        nextblockhash: '-',
        transactions: [],
        //part4
        fee: '-',
        blockindex: '-',
        blocktime: '-',
        txid: '-',
        timereceived: '-',
        comment: '-',
        hex: '-',

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
        })
      },

      getBlockchainInfo() {
        this.requestRpc('getblockchaininfo', null, (result) => {
          this.blockCount = result.blocks;
          this.currentDifficulty = result.difficulty;

          this.getRecentBlocks();
          this.getBlockDetail(this.blockCount);
          this.getTransactionsDetail(this.blockCount);
        });
      },

      getRecentBlocks() {
        let times = this.blockCount;

        for (let i = 0; i < 5; i++) {
          if (times - i < 0) break;
          this.requestRpc('getblockhash', [times - i], (result) => {
            this.blockList.push({
              'height': times - i,
              'hash': result
            });
          })
        }

      },


      getBlockDetail(height) {
        this.requestRpc('getblockhash', [height], (result) => {
          this.requestRpc('getblock', [result], (result) => {
            this.height = result.height;
            this.confirmations = result.confirmations;
            this.size = result.size;
            this.time = result.time;
            this.nonce = result.nonce;
            this.bits = result.bits;
            this.difficulty = result.difficulty;
            this.merkleroot = result.merkleroot;
            this.hash = result.hash;
            this.previousblockhash = result.previousblockhash;
            this.nextblockhash = result.nextblockhash;
            this.transactions = result.tx;
          });
        })
      },

      getTransactionsDetail(height) {
        this.requestRpc('getblockhash', [height], (result) => {
          this.requestRpc('getblock', [result], (result) => {
            // this.requestRpc('gettransaction', [result.tx[0]], (result) => { //todo replace this line
            this.requestRpc('gettransaction', ['82cb4fb99122133604e91a2ff2120f1baf0825f3f5d4a9d691a5efbcdd1d9372'], (result) => {
              console.log(result.comment)
              this.fee = result.fee;
              this.blockindex = result.blockindex;
              this.blocktime = result.blocktime;
              this.txid = result.txid;
              this.timereceived = result.timereceived;
              this.comment = result.comment;
              this.hex = result.hex;

              //todo detail
            })
          })
        })
      }
    },
    mounted() {
      this.getBlockchainInfo();
    }
  }
</script>

<style>
  h3 {
    margin: 12px 0;
  }

  .base-info {
    display: flex;
    justify-content: space-between;
    color: dimgray;
    margin: 5px 0;
  }

  .base-info span:first-child {
    font-weight: bolder;
  }


</style>
