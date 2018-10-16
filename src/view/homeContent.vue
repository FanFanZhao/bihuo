<template>
    <div class="indexBlackes">
        <!-- <div class="banneres">
            <div class="content ">
                <h1>区块链数字资产交易平台</h1>
                <p class="introduce">安全&nbsp;&nbsp;&nbsp;公正&nbsp;&nbsp;&nbsp;高效&nbsp;&nbsp;&nbsp;不可篡改</p>
                <div class="QRCode"><img src="@/assets/images/code.png"></div>
                <p class="kefu">手机下载二维码</p>
            </div>
        </div> -->
        <!-- <home-login></home-login> -->
        <home-login></home-login>
        <div class="swiper-container banner_wrap swiper-container-horizontal">
            <div class="swiper-wrapper">
               <div class="swiper-slide sliders">
                   <a href="">
                   <img src="../../static/imgs/bnr_dcr_cn.png" />
                   </a>
               </div>
                <div class="swiper-slide sliders">
                   <a href="">
                   <img src="../../static/imgs/bnr_lend2_cn.jpeg" />
                   </a>
               </div>
               
            </div>
             <div class="swiper-pagination swiper-pagination02"></div>
        </div>
        <!-- <div class="carousel">
            <div class="swiper-container swiper-container01 swiper-container-horizontal">
                <div class="swiper-wrapper">
                    <div class="swiper-slide" :class="{active:index==curSwiper}" v-for="(item ,index) in swiperList" style="width: 232px; margin-right: 10px;" @mouseover="mouseEnter(index)">
                        <p class="name">{{item.symbol}}</p>
                        <p class="last">{{item.quotes.USD.price}}</p>
                        <p class="range" :class="{red:item.quotes.USD.percent_change_24h>=0}">
                            <i class="iconfont icon-u-arrow3-loss" v-if="item.quotes.USD.percent_change_24h<0"></i>
                            <i class="iconfont icon-u-arrow3-right" v-else></i>
                            {{item.quotes.USD.percent_change_24h}}%
                        </p>
                    </div>
                </div>   
                <div class="swiper-pagination01"></div>
            </div>
            <div slot="button-prev" class="swiper-button-prev" tabindex="0" role="button" aria-label="Previous slide"></div>
            <div slot="button-next" class="swiper-button-next" tabindex="0" role="button" aria-label="Next slide"></div>
        </div> -->
        <!-- <div class="notice">
           <ul class="flex alcenter around notice_ul">
               <li v-for="item in noticeList" :key="item.id" class="fl notice_li flex1" style="color: #cdd6e4;"><a class="notice_a ft12" :data-id='item.id'>{{item.name}}</a></li>
           </ul>
        </div> -->
        <!-- <div class="active-data clearfix">
            <div class="data high">
                <div class="name">最高价</div>
                <div class="content">{{coinKline.hight}}</div>
            </div>
            <div class="data high">
                <div class="name">最低价</div>
                <div class="content">{{coinKline.low}}</div>
            </div>
             <div class="data high">
                <div class="name">开盘价</div>
                <div class="content">{{coinKline.open}}</div>
            </div>
            <div class="data high">
                <div class="name">收盘价</div>
                <div class="content">{{coinKline.close}}</div>
            </div>
            <div class="data range">
                <div class="name">涨跌幅</div>
                <div class="content">-{{Math.floor(((coinKline.close-coinKline.open)/coinKline.open) * 100)/100 || 0}}%</div>
            </div>
            <div class="data vol">
                <div class="name">成交量</div>
                <div class="content">{{coinKline.volume}}</div>
            </div>
            <div class="time">24H</div>
        </div> -->
        <!-- <div id="chart" _echarts_instance_="ec_1533699609264" style="width: 100%; height: 320px; -webkit-tap-highlight-color: transparent; user-select: none; position: relative; background: transparent;">
            
        </div> -->
          <div class="coin-tab">
            <ul class="coins">
              <li v-for="(coin,index) in quotation" :key="index" @click="nowCoin = coin.name" :class="{activeCoin:nowCoin == coin.name}">对{{coin.name}}交易区<span class='arrow' v-if="nowCoin == coin.name"></span></li>
            </ul>
          </div>
        <div class="coins-list">
          <div class="list-title">
            <span>币种对{{nowCoin}}</span>
            <span>价格({{nowCoin}})</span>
            <span>交易量({{nowCoin}})</span>
            <span>涨跌</span>
          </div>
          
          <ul class="list-con scroll" v-for="(item,index) in quotation" :key="index" v-if="nowCoin == item.name">
            <li v-for="(li,inde) in item.quotation" :key="inde" :data-name='item.name+"/"+li.name'>
              <div class="two-coin">
                
                <span style="color:#61688a;font-weight:bold">{{li.name}}</span>
              </div>
              <div class="yester">
                <!-- <span :class="setColor(li.change)">{{li.now_price==null?'0':li.now_price}}</span>/ -->
                <span style="color:#666">{{li.now_price==null?'0':li.now_price}}</span>
              </div>
              <div class="count">{{li.volume == null?'0':li.volume}}</div>
              <div class="yes-toa">
                <!-- <span :class="setColor(li.last_price,li.yesterday_last_price)">{{li.change == null?'+0.000':li.change}}%</span> -->
                <span :class="setColor(li.change)">{{(li.change>0?'+':'')+(li.change-0).toFixed(2)}}%</span>
              </div>
            </li>
          </ul>
        </div>
        <div class="coinTable" style='display:none'>
            <div class="tabhang">
                <div class="tabul">
                    <ul class="clearfix">
                        <li :class="{active:index==curCoinTab}" v-for="(tab,index) in quotation" :key="index" @click="getCurrent(index)" >{{tab.name}}</li> 
                    </ul>
                </div>
                <div class="tabtable">
                    <ul class="table-nav">
                        <div class="nav_left">
                        <li style="width:220px;text-align:left;">交易市场</li>
                        <!-- <li>最新成交价</li>
                        <li>涨跌</li> -->
                        <li style="width:220px;text-align:center;">最高价 ( 24h )</li>
                        <li style="width:220px;text-align:center;">最低价 ( 24h )</li>
                        <li style="width:220px;text-align:center;">最新价</li>
                        <!-- <li>成交量</li> -->
                        </div>
                        <li class="last tc fr">操作</li>
                    </ul>
                    <div class="scroll list-list">
                      <ul class="content" v-for="(coin,index) in coin_list">
                          <div class="con_left">
                          <li class="hovertd" style="width:220px;text-align:left;">
                              <i class="iconfont icon-BTCUSDT-copy" style="float:initial;"></i>
                              <!-- <span class="hover">{{coin.symbol}}</span><br> -->
                              <span>{{coin.name}}</span>
                          </li>
                          <!-- <li> -->
                              <!-- <span >¥ {{coin.quotes.USD.price}}</span> -->
                              <!-- <span>{{coin.name}}</span> -->
                              <!-- / ¥ {{coin.quotes.USD.price}} -->
                              <!-- <i class="iconfont icon-arrow-down"></i> -->
                          <!-- </li> -->
                          <li style="width:220px;text-align:center;">{{coin.min_price}}</li>
                          <li style="width:220px;text-align:center;">{{coin.max_price}}</li>
                          <li style="width:220px;text-align:center;">{{coin.new_price}}≈0.00CNY</li>
                          <!-- <li class="red" :class="{green:coin.quotes.USD.percent_change_24h.toString().substr(0, 1)=='-'}">
                              <i v-if="coin.quotes.USD.percent_change_24h.toString().substr(0, 1)=='-'" class="iconfont icon-arrow-down"></i>
                              <i v-else class="iconfont icon-arrow-up"></i>
                              {{coin.quotes.USD.percent_change_24h}}%
                              </li> -->
                          <!-- <li>{{coin.highest}}</li>
                          <li>{{coin.lowest}}</li> -->
                          <!-- <li>{{coin.total_supply}}</li> -->
                          </div>
                          <li class="last fr">
                              <a class="baseColor">充币</a>
                              <a class="baseColor withdraw">提币</a>
                              <a class="baseColor">兑换</a>
                              <!-- <button>交易</button> -->
                          </li>
                      </ul>
                    </div>
                    
                </div>
            </div>
        </div>
        <!-- <div class="bottom">
            <p>温馨提示</p>
            <p>数字资产是创新的投资产品，价格波动较大，具有较高的投资风险，请您投资前 对数字资产充分认知，理性判断自己的投资能力，审慎做出投资决策。</p>
        </div> -->
    </div>
    
</template>


<script>
import "@/assets/style/iconfont2.css";
import "@/assets/style/iconfont.css";
import "@/assets/style/index.css";
import Swiper from "swiper";
import "swiper/dist/css/swiper.min.css";
import indexHeader from "@/view/indexHeader";
import homeLogin from "@/view/homeLogin";
// var echarts = require("echarts");
export default {
  name: "homeContent",
  components: { indexHeader,homeLogin },
  data() {
    return {
      quotation: [],
      nowCoin: "",
      //   banner_imgs:[
      //       {href:'',img:'../assets/images/bg2.png'},
      //       {href:'',img:'../assets/images/bg2.png'},
      //       {href:'',img:'../assets/images/bg2.png'}
      //   ],
      curSwiper: 0,
      curCoinTab: 0,
      coinTabList: [{ title: "USDT行情" }, { title: "BTC行情" }],
      coinKlineList: [],
      coinKline: {},
      swiperList: [
       
      ],
      coinList: [
        
      ],
      coin_list: [
      ]
    }
  },
  created() {
    // this.init(this.initKline);
    this.getQuotation();
  },
  mounted() {
    var mySwiper = new Swiper(".swiper-container01", {
      // 如果需要分页器
      pagination: ".swiper-pagination01",
      paginationClickable: true,
      // 如果需要前进后退按钮
      slidesPerView: 5,
      nextButton: ".swiper-button-next",
      prevButton: ".swiper-button-prev",
      observer: true, //修改swiper自己或子元素时，自动初始化swiper
      observeParents: true //修改swiper的父元素时，自动初始化swiper
    });
    var mySwiper02 = new Swiper(".banner_wrap", {
      // direction: 'vertical',
      loop: true,
      autoplay: 2000,
      // 如果需要分页器
      pagination: ".swiper-pagination02",
      paginationClickable: true,
      observer: true, //修改swiper自己或子元素时，自动初始化swiper
      observeParents: true //修改swiper的父元素时，自动初始化swiper
    });
    // this.setChart();
    this.$http({
      url: this.$utils.laravel_api + "news/help",
      method: "post",
      data: {}
    }).then(res => {
        console.log(res);
        if (res.status === 200) {
          this.noticeList = res.data.message;
        } else {
          layer.msg(res.message);
        }
    })
    .catch(error => {
      console.log(error);
    });
    //  eventBus.$on('toNew', function (data) {
    //   console.log(data);
    //   if(data){
    //         var newprice=data.newprice;
    //         var cname=data.istoken
    //         console.log(that.currency_name) 
    //         console.log(newprice)
    //         $("span[data-name='"+cname+"']").html('$'+newprice);
    //     }
    // }); 
    this.connect();
       
  },
  methods: {
    connect() { 
      var that=this;
      console.log('socket')
      that.$socket.emit("login", localStorage.getItem('user_id'));
      that.$socket.on("transaction", msg => {
        console.log(msg);
        var cname=msg.token;
        var yesprice = msg.yesterday;
        var toprice = msg.today;
        console.log(cname)
        var zf=0;
        if((toprice-yesprice )==0){
            zf='0%'
        }else if(toprice==0){
            zf='-100'
        }else if(yesprice){
            zf="+100%"
        }else{
          zf=((toprice-yesprice)/yesprice/100).toFixed(2);
          if(zf>0){
            zf = '+'+zf+ '%';
          } else {
            zf = zf+'%'
          }
        }
          var zf=toprice-yesprice
          $("li[data-name='"+cname+"']").find('.yester span').html(yesprice);
          $("li[data-name='"+cname+"']").find('.today span').html(toprice);
          $("li[data-name='"+cname+"']").find('.yes-toa span').html(zf);
      });
    },
    setColor(c){
      if(c>0){
        return 'ceilColor';
      } else if(c<0){
        return 'redColor';
      } else {
        return '';
      }
    },
    getQuotation() {
      this.$http({
        url: "/api/currency/quotation",
        method: "get"
      }).then(res => {
        console.log(res.data);
        if (res.data.type == "ok" && res.data.message.length != 0) {
          this.quotation = res.data.message;
          this.nowCoin = this.quotation[0].name;
        }
      });
    },
    getCurrent(index) {
      this.curCoinTab = index;
    },
    mouseEnter(index) {
      this.curSwiper = index;
    },
    init(callback) {
      this.$http.post(this.$utils.laravel_api + "quotation").then(res => {
        if (res.data.type == "ok") {
          this.coinList = res.data.message.coin_list;
          this.swiperList = res.data.message.coin_list;
          callback && callback();
        } else {
          // layer.msg(res.message);
        }
      });
    },
    initKline() {
      this.$http.post(this.$utils.laravel_api + "historical_data").then(res => {
        if (res.data.type == "ok") {
          if (res.data.message.day.length > 0) {
            this.coinKline = res.data.message.day[0].data;
            this.coinKlineList = res.data.message.day;
          }
          console.log(res.data.message.day[0].data);
          console.log(res.data.message.day);
        }
      });
    },
    timestampToTime(timestamp) {
      var date = new Date(timestamp);
      let Y = date.getFullYear() + "/";
      let M =
        (date.getMonth() + 1 < 10
          ? "0" + (date.getMonth() + 1)
          : date.getMonth() + 1) + "/";
      let D = date.getDate() + " ";
      return Y + M + D;
    },

    setChart() {
      var myChart = echarts.init(document.getElementById("chart"));
      var that = this;
      // 指定图表的配置项和数据
      var base = 2;
      var date = [];
      var data = [1, 2, 4, 5, 6, 7, 8];
      var now = new Date();
      console.log(that.coinKlineList);
      for (var i in that.coinKlineList) {
        let temp = that.timestampToTime(that.coinKlineList[i].timestamp);
        console.log(temp);
        date.push(temp);
      }
      console.log(date);
      var option = {
        textStyle: {
          color: "#d45858"
        },
        backgroundColor: "#fff",
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            label: {
              backgroundColor: "#fff"
            }
          }
        },
        title: {
          left: "center",
          text: "24小时数据图"
        },
        grid: {
          left: "3%",
          right: "3%",
          bottom: "5%",
          containLabel: true
        },
        toolbox: {
          feature: {
            // dataZoom: {
            //     yAxisIndex: 'none'
            // },
            // restore: {},
            saveAsImage: {}
          }
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: date,
          color: ["#ff9232"]
        },

        yAxis: {
          type: "value",
          yaxisLabel: {
            backgroundColor: "#fff"
          }
        },
        series: [
          {
            name: "当前价格",
            type: "line",
            smooth: true,
            symbol: "none",
            sampling: "average",
            itemStyle: {
              normal: {
                // 6e446e
                color: "#6e446e"
              }
            },
            areaStyle: {
              normal: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  {
                    offset: 0,
                    color: "#6e446e"
                  },
                  {
                    offset: 1,
                    color: "#1e1e2a"
                  }
                ])
              }
            },

            data: data
          }
        ]
      };

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    },
    // go_detail(index,inde){
    //   this.$router.push({
    //     path:'/dealCenter',
    //     name:'dealCenter',
    //     params:{
    //       legal_index:index,
    //       currency_index:inde
    //     }
    //   })
    // }
  }
};
</script>
<style lang='scss' scoped>
.swiper-container{
  height: 310px;
}
.swiper-container a{
  display: block;
  height: 310px;
}
.swiper-container img{
  display: block;height: 310px;
}
  .coin-tab {
    line-height: 52px;
    height: 52px;
    background: #252e3e;
    padding:  0 50px;
    // color: #c7cce6;
    display: flex;
    > ul {
      width: 1280px;
      display: flex;
      margin: 0 auto;
      li {
        padding: 0 40px;
        color:#ddd;
        // box-shadow: 0 0 1px hsla(231, 9%, 54%, 0.2);
       
      }
      .activeCoin {
        border-bottom: none;
        color:#d45858;
      }
    }
  }
/* 币种列表 */
.coins-list {
  margin: 10px auto;
  max-width: 1280px;
  line-height: 51px;
  text-align: center;
  
  .list-title {
    display: flex;
    padding: 0 30px;
    > span {
      flex: 1;

      text-align: center;
      // color: #c7cce6;
      font-size: 14px;
    }
    >span:first-child{
        text-align: left;
      }
      >span:last-child{text-align: right}
  }
  .list-con {
    // background: rgb(32, 36, 55);
    max-height: 680px;
    overflow: scroll;
    
    li {
      display: flex;
      border-top: 1px solid #ddd;
      padding: 0 30px;
      
      // color: #c7cce6;
      > div {
        flex: 1;
        text-align: center;
      }
      >div:first-child{
        text-align: left;
      }
      >div:last-child{text-align: right}
    }
  }
}
.carousel .swiper-slide:hover {
  background-color: #327add;
}
.nav_left,
.con_left {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.withdraw {
  margin: 0 4px;
}
.carousel .last a:hover {
  cursor: pointer;
}
.sliders {
  width: 100%;
}
.sliders img {
  width: 100%;
  // height: 500px;
}
.notice_ul {
  padding: 25px 0;
  // background: #161923;
  margin-bottom: 5px;
}
.notice_li {
  flex: 1;
  text-align: center;
}
.notice_li::after {
  content: "/";
  float: right;
}
.notice_li:last-child:after {
  content: "";
  color: #6b80ae;
}
.notice_a:hover {
  color: #6b80ae;
  cursor: pointer;
}
.coins li{
  position: relative;
}
.arrow{
    border-width: 16px;
    border-right: 8px solid transparent;
    border-left: 8px solid transparent;
    border-top: none;
    border-bottom: 8px dashed;
    position: absolute;
    bottom: 0;
    left: 50%;
    margin-left: -4px;
}
.coins li:hover{
  cursor: pointer;
  background: #303e4c;
}
</style>


