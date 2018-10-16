<template>
        <div class="klines">
          <div class="top-txt ft14">{{leg_name}}/{{currency_name}}</div>
          <div class="kline">
            <div id="kline_container"></div>
          </div>
          
        </div>
      </template>
      
      <script>
export default {
  name: "kline",
  data() {
    return {
      legal_id: 3,
      currency_id: 2,
      leg_name: "HQ",
      currency_name: "BTC"
    };
  },
  created() {
    this.token = localStorage.getItem("token") || "";
  },
  methods: {
    king() {
      var _that = this;
      var url = "timeshar";
      var kline = new Kline({
        element: "#kline_container",
        width: $(".kline").width(),
        height: 490,
        theme: "light", // light/dark
        language: "zh-cn", // zh-cn/en-us/zh-tw
        ranges: ["line", "1m", "5m", "15m", "30m", "1h", "1d", "1w"],
        symbol: "BTC/coin4",
        symbolName: "COIN5_COIN4",
        type: "poll", // poll/socket
        //   url: "../../static/lib/mock.json",
        //   datas:{id:123},
        url:  + "/api/currency/" + url,
        limit: 1000,
        intervalTime: 20000,
        debug: true,
        showTrade: true,
        currency_id: this.currency_id,
        legal_id: this.legal_id,
        onResize: function(width, height) {
          // console.log("chart resized: " + width + " " + height);
        }
      });
      kline.draw();
    },

    getData() {
      var that = this;
      this.$http({
        url: "/api/" + "deal/info",
        method: "post",
        data: {
          legal_id: that.legal_id,
          currency_id: that.currency_id,
          type: 5
        },
        headers: { Authorization: that.token }
      })
        .then(res => {
          console.log(res);
          if (res.data.type == "ok") {
          } else {
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted() {
    var that = this;
    this.king();
    // setInterval(function(){
    that.king();
    // },2000)
    eventBus.$on("toTrade", function(data) {
      $("#kline_container").html("");
      console.log(data);
      if (data) {
        (that.currency_id = data.legal_id), (that.legal_id = data.currency_id);
        (that.currency_name = data.currency_name),
          (that.leg_name = data.leg_name);
        that.king();
      }
    });
  }
};
</script>
      
      <!-- Add "scoped" attribute to limit CSS to this component only -->
      <style scoped>
.top-txt {
  height: 40px;
  line-height: 40px;
  /* background: #181b2a;
        -webkit-box-shadow: 0 2px 6px rgba(0,0,0,.1);
        box-shadow: 0 2px 6px rgba(0,0,0,.1); */
  padding: 0 15px;
}
.kline {
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  height: 490;
  position: relative;
}
</style>
      