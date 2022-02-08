<template>
  <div id="app">
    <div class="header"></div>
    <div class="body">
      <!-- <select id="area" class="areaA" v-model="nowVal">
          <option value="中正區">中正區</option>
          <option value="萬華區">萬華區</option>
          <option value="大同區">大同區</option>
          <option value="中山區">中山區</option>
          <option value="松山區">松山區</option>
          <option value="大安區">大安區</option>
          <option value="信義區">信義區</option>
          <option value="內湖區">內湖區</option>
          <option value="南港區">南港區</option>
          <option value="士林區">士林區</option>
          <option value="北投區">北投區</option>
          <option value="文山區">文山區</option>
          <option value="文山區">選擇區域</option>

        </select> -->
      <div class="up">
        <input
          id="area"
          type="text"
          list="typelist"
          placeholder="區域"
          v-model="nowVal"
          v-on:click="clearVal"
          class="search_area"
        />
        <datalist id="typelist">
          <option value="中正區">中正區</option>
          <option value="萬華區">萬華區</option>
          <option value="大同區">大同區</option>
          <option value="中山區">中山區</option>
          <option value="松山區">松山區</option>
          <option value="大安區">大安區</option>
          <option value="信義區">信義區</option>
          <option value="內湖區">內湖區</option>
          <option value="南港區">南港區</option>
          <option value="士林區">士林區</option>
          <option value="北投區">北投區</option>
          <option value="文山區">文山區</option>
        </datalist>
        <input
          class="search_addr"
          type="text"
          placeholder="地址、路段"
          v-model="nowValB"
        />
        <button class="search btn" v-on:click="getData">Search</button>
        <button class="clear btn" v-on:click="clearVal">Clear</button>
      </div>
      <br />
      <div class="info">
          <div v-for="(data, index) in filteredStop" :key="index" class="container">
            <div class="stop">{{data.sna.replace("YouBike2.0_", "")}}</div><br>
            <div class="stopArea">{{data.sarea}}</div><br>
            <div class="stopAddr">{{data.ar}}</div>
            <div class="nowData">
              <div class="total">總車輛數：{{data.tot}}</div>
              <div class="left">空置車輛：{{data.sbi}}</div>
            </div>
          </div>
      </div>
    </div>
  </div>
</template> 

<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      nowVal: "區域",
      youbikeData: [],
    };
  },
  methods: {
    clearVal: function () {
      this.nowVal = "";
      this.nowValB = "";
    },
    getData() {
      axios
        .get(
          "https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json"
        )
        .then((res) => {
          console.log(res);
          this.youbikeData = res.data;
        });
    },
  },
  computed: {
    filteredStop() {
      if (this.youbikeData.length== 0) return []
      return this.youbikeData.filter((stop) => {
        if (this.nowVal=="") return stop.ar.includes(this.nowValB);
        return stop.sarea.includes(this.nowVal)&&
               stop.ar.includes(this.nowValB);
      });
    },
  },
};
</script>

<style lang="scss">
.up {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;

  .search_area {
    width: 100px;
    height: 20px;
  }
  .search_addr {
    width: 400px;
    height: 20px;
  }
  .btn {
    height: 25px;
  }
}
.info {
  display: flex;
  flex-direction:row;
  position: relative;
  background-color: red;
  padding: 4px;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  .container {
    position: relative;
    background-color: yellow;
    width: 350px;
    margin: 5px;
    border-radius: 8px;
    display: flex;
    flex-direction:column;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    &:hover {
      bottom: 2px;
    }
    .stop {
      margin-top: 10px;
      .stopName {
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .stopArea {
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .stopAddr {
        display: flex;
        justify-content: center;
        align-items: center;
      }
    }
  }
  .nowData {
    margin: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
    .total {
      margin-right: 50px;
    }
  }
}
</style>
