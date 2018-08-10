<template>
  <div class='main'>
    <h2>Base Markets</h2>
    <ul class='menu'>
      <li>
        <a
          href="#" @click="event => filter(event, 'BTC')"
        >
          BTC
        </a>
      </li>
      <li>
        <a
          href="#" @click="event => filter(event, 'USDT')"
        >
          USDT
        </a>
      </li>
      <li>
        <a
          href="#" @click="event => filter(event, 'NZDT')"
        >
          NZDT
        </a>
      </li>
      <li>
        <a
          href="#" @click="event => filter(event, 'LTC')"
        >
          LTC
        </a>
      </li>
      <li>
        <a
          href="#" @click="event => filter(event, 'DOGE')"
        >
          DOGE
        </a>
      </li>
    </ul>

    <ul v-if='coins.length > 0' class='coins-list'>
      <li v-for='coin in filteredCoins' :key='coin.Id'>
        <coin :coin='coin' :market='coinMarketData(coin.Symbol)' />
      </li>
    </ul>
  </div>
</template>

<script>
import $ from "jquery";
import coin from "./coin";

export default {
  name: "Home",
  created() {
    $.ajax({
      type: "GET",
      url: "https://www.cryptopia.co.nz/api/GetCurrencies",
      dataType: "html",
      success: data => {
        let api_result = JSON.parse(data);
        this.coins = api_result.Data;
        //console.log(this.coins);
      }
    });
    $.ajax({
      type: "GET",
      url: "https://www.cryptopia.co.nz/api/GetMarkets/",
      dataType: "html",
      success: data => {
        let api_result = JSON.parse(data);
        this.tradePairs = api_result.Data;
        //console.log(this.tradePairs);
        let markets = {};
        for (let tradePair of this.tradePairs) {
          let [coin, base] = tradePair.Label.split("/");
          let hasBaseMarket = !!markets[base];
          if (!hasBaseMarket) {
            markets[base] = {};
          }
          markets[base][coin] = tradePair;
        }
        this.markets = markets;
      }
    });
  },
  methods: {
    filter: function(event, baseMarketCoin) {
      this.currentBaseMarket = baseMarketCoin;
    },
    coinMarketData(symbol) {
      if (this.currentBaseMarket) {
        let result = this.markets[this.currentBaseMarket][symbol];
        result.currentBaseMarket = this.currentBaseMarket;
        return result;
      } else {
        return {};
      }
    }
  },
  computed: {
    filteredCoins() {
      if (this.currentBaseMarket == null) {
        /* Vue adds a hook on every .this */
        return this.coins;
      }
      return this.coins.filter(coin => {
        if (!!this.markets[this.currentBaseMarket]) {
          if (!!this.markets[this.currentBaseMarket][coin.Symbol]) {
            return true;
          }
          return false;
        }
      });
    }
  },
  components: {
    coin
  },
  data() {
    return {
      currentBaseMarket: null,
      markets: {},
      tradePairs: [],
      coins: []
    };
  }
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
.menu li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.coins-list {
  background-color: #fff;
  max-width: 550px;
  margin: 0 auto;
  padding: 1em;
}
.coins-list li {
  color: #2c3e50;
  text-align: left;
}
</style>
