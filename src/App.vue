<template>
  <div id="app">
    <div class="header">
      <div class="headerT">YouBike2.0 Map</div>
      <div class="headerW">台北市Youbike2.0即時資訊</div>
      <br>

    </div>
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
      <div class="info" v-if="this.start==false">
        <div class="alert" v-if="this.start==false&&filteredStop==''">很抱歉，您目前的搜尋沒有結果</div>
        <div v-for="(data, index) in filteredStop" :key="index" class="container" v-on:click="toMap(data.ar)">
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
     <div class="footer">
      Made by Kevin Chang
    </div>
  </div>
</template> 

<script>
import axios from "axios";

export default {
  name: "app",
  data() {
    return {
      nowVal: "",
      youbikeData: [],
      start: true,
    };
  },
  methods: {
    clearVal() {
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
      this.start = false
    },
    toMap(address) {
      // const arr = this.data.ar
      window.open(`https://www.google.com/maps/?q=${address}`)
    }
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
html {
  background-color: #3f4223;
}
.header {
  background-color: #42233f;
  color: #fff;
  margin-bottom: 20px;
  padding-right: 20px;
  .headerT {
    font-size: 50px;
    font-weight: 100;
    position: relative;
    display: flex;
    justify-content: right;
  }
  .headerW {
    font-size: 20px;
    font-weight: 100;
    position: relative;
    display: flex;
    justify-content: right;
  }
}
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
  background-color: #42233f;
  padding: 4px;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  color: #3f4223;
  font-weight: bold;
  .alert {
    color: #fff;
    margin-bottom: 20px;
    padding-right: 20px;
    padding-top: 20px;
  }
  .container {
    position: relative;
    background-color: #c798c3;
    width: 350px;
    margin: 5px;
    border-radius: 8px;
    display: flex;
    flex-direction:column;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
    cursor: pointer;

    &:hover {
      bottom: 3px;
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
.footer {
    background-color: #42233f;
    color: #fff;
    position: relative;
    display: flex;
    justify-content: center;
    bottom: 0;
    width: 100%;
  }
</style>
