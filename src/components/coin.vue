<template>
    <div class="coin">
        <img :src="imageUrl" />
        <h3>{{ coin.Symbol }}</h3>
        <p>{{ coin.Name }}</p>
        <p v-if="market.currentBaseMarket">
          <span v-if="market.Change > 0.5" class="success">{{ market.Change }}%</span>
          <span v-if="market.Change < -0.5" class="error">{{ market.Change }}%</span>
        </p> <!-- since you last checked, localStorage -->
        <div class="coininfo">
          <span class="noconnections" v-if="coin.Status == 'NoConnections'"><b>📶&nbsp;No Connections</b> - <a href="https://support.cryptopia.co.nz/csm/?id=csm_get_help&sys_id=76c9390cdb8597808653787dbf961934">provide some?</a></span>
          <span class="maint" v-if="coin.StatusMessage"><b>🔧&nbsp;In maintenance:</b>  {{ coin.StatusMessage }}</span>
          <span v-else class="neutral">✅ Functional</span>
          <span class="delisting" v-if="coin.ListingStatus != 'Active'">🗑️&nbsp;{{ coin.ListingStatus }} - Market/s Closing</span>
          <span v-else class="neutral">✅ Listed</span>
          <a :href="'https://www.cryptopia.co.nz/CoinInfo/?coin=' + coin.Symbol">- 📜&nbsp;View CoinInfo</a>
          <a v-if="market.currentBaseMarket" :href="'https://www.cryptopia.co.nz/Exchange/?market=' + coin.Symbol + '_' + market.currentBaseMarket">📈&nbsp;View&nbsp;Market&nbsp;({{ market.Label }})</a>
          <span v-else>🤷&nbsp;No&nbsp;Market&nbsp;Selected</span>
          <span v-if="market.currentBaseMarket">
            <b>Quickie Market Details...</b> 
            required deposit confs: {{ coin.DepositConfirmations }}, 
            last price: {{ market.LastPrice }}, 
            low: {{ market.Low }}, 
            high: {{ market.High }}. 
            <i>All prices are in {{ market.currentBaseMarket }}.</i>
            </span>
        </div>
    </div>
</template>

<script>
export default {
  props: {
    coin: {
      type: Object,
      required: true
    },
    market: {
      type: Object,
      required: true
    }
  },
  computed: {
    imageUrl () {
      return 'https://www.cryptopia.co.nz/Content/Images/Coins/' + this.coin.Symbol + '-medium.png'
    }
  }
}
</script>

<style scoped>
div.coin {
  padding: 1em 0;
  border-bottom: 1px dotted #ccc;
}
div.coininfo {
  margin-top: 1em;
}
img {
  vertical-align: middle;
}
h3, p {
  font-size: 1.35em;
  display: inline-block;
  vertical-align: middle;
  margin: 0 5px;
}
p {
  font-size: 1.15em;
}
a, span {
  text-decoration: none;
  margin: 2px;
  margin-left: 0;
  display: inline-block;
  padding: 2px;
}
.maint {
  background-color: #ffc107;
  display: block;
}
.noconnections {
  background-color: #fd7e14;
}
.delisting, .error {
  background-color: #dc3545;
  color: #fff;
}
.success {
  background-color: #28a745;
  color: #fff;
}
</style>
