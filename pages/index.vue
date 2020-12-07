<template>
  <div style="width: 80%;margin: 0 auto">

    <h1 style="margin-top: 20px">Block Explorer</h1>

    <el-divider></el-divider>

    <div style="display: flex;align-items: start">

      <div style="width: 30%;display: inline-block">
        <h3>Network State</h3>

        <div style="width: 300px">
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

      <div style="width: 70%;display: inline-block">
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

    <div>
      <h3>Block Detail</h3>

      <div style="width: 50%">
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
          });
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
