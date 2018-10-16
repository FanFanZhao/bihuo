<template>
    <div class="entrust">
        <div class="title fColor1">
            <div class="tab_title">
                <span v-for="(url,index) in urlList" :class="{'active': index == isUrl}" @click="changeType(index,url.url)">{{url.title}}</span>
            </div>
            <div class="tab_title fr ft12" v-if="isUrl == 0">
                <span v-for="(way,index) in wayList" :class="{'active': index == isChoosed}" @click="wayChoose(index,way.type)">{{way.title}}</span>
            </div>
        </div>
        <div class="content" v-show="isUrl==0" style="border:1px solid #ccc">
            <ul class="list-title fColor2 ft12 clear">
                <li class="fl w20">时间</li>
                <li class="fl w12">交易对</li>
                <li class="fl w12">方向</li>
                <li class="fl w12">价格</li>
                <li class="fl w14">数量</li>
                <li class="fl w20">委托总额</li>
                <li class="fl w8 tr">操作</li>
            </ul>
            <div class="container scroll" v-if="enList.length>0">
                <ul class="list-item ft12">
                    <li v-for="item in enList" class="clear">
                        <span class="fl w20">{{item.create_time}}</span>
                        <span class="fl w12">{{item.currency_name}}/{{item.legal_name}}</span>
                        <span class="fl w12" :class="{'green': item.typeInfo == 'in'}">{{item.typeInfo=='in'?'买入':'卖出'}}</span>
                        <span class="fl w12">{{item.price}}</span>
                        <span class="fl w14">{{item.number}}</span>
                        <span class="fl w20">{{item.price * item.number}}</span>
                        <span class="fl w8 tr curPer ceilColor" @click="revoke(item.id)">撤销</span>
                    </li>
                </ul>
                <div class="getmore tc fColor1 ft14 mt10 curPer" @click="getMore" v-if="!loading && enList.length>9">{{more}}</div>
                <div class="tc" v-if="loading">
                    <img src="@/assets/images/loading.gif" alt=""  class="lodw20">
                    <p class="ft12 baseColor">加载中...</p>
                </div>
            </div>
            <div class="no_data tc" v-if="enList.length<=0">
                <img src="@/assets/images/nodata.png" alt="">
                <p class="fColor2 ft18">暂无数据</p> 
            </div>
        </div>
        <div class="content" v-show="isUrl==1">
            <ul class="list-title fColor2 ft12 clear">
                <li class="fl w20">时间</li>
                <li class="fl w10">交易对</li>
                <li class="fl w8">数量</li>
                <li class="fl w10">价格</li>
                <li class="fl w8">委托总额</li>
                <li class="fl w10">成交均价</li>
                <li class="fl w10">状态</li>
                <li class="fl w10">手续费</li>
                <li class="fl w8 tr">方向</li>
            </ul>
            <div class="container scroll" v-if="hisList.length>0">
                <ul class="list-item fColor1 ft12">
                    <li v-for="item in hisList" class="clear">
                       <span class="fl w20">{{item.time}}</span>
                        <span class="fl w10">{{item.currency_name}}/{{item.legal_name}}</span>
                        <span class="fl w8">{{item.number}}</span>
                        <span class="fl w10">{{item.price}}</span>
                        <span class="fl w8">{{(item.price * item.number) | numFilter}}</span>
                        <span class="fl w10">{{item.price}}</span>
                        <span class="fl w10">已成交</span>
                        <span class="fl w10">{{item.type=='in'? item.in_fee:item.out_fee}}</span>
                        <span class="fl w8 tr" :class="item.type=='out'?'redColor':''">{{item.type=='in'?'买入':'卖出'}}</span>
                    </li>
                </ul>
                <div class="getmore tc fColor1 ft14 mt10 curPer" @click="getMore01" v-if="!loading && hisList.length>9">{{more01}}</div>
                <div class="tc" v-if="loading">
                    <img src="@/assets/images/loading.gif" alt="" class="lodw20">
                    <p class="ft12 baseColor">加载中...</p>
                </div>
            </div>
            <div class="no_data tc" v-if="hisList.length<=0">
                <img src="@/assets/images/nodata.png" alt="">
                <p class="fColor2 ft18">暂无数据</p>   
            </div>
        </div>
    </div>
</template>
<script>

export default {
    name:"entrust",
    data (){
        return{
            address:'',
            isChoosed:0,
            isUrl:0,
            page:1,
            page01:1,
            url:'entrust',
            type:'in',
            more:'加载更多',
            more01:'加载更多',
            loading:false,
            urlList:[{title:"当前委托",url:'transaction_in'},{title:"历史委托",url:"transaction_complete"}],
            wayList:[{title:"买入",type:"in"},{title:"卖出",type:"out"}],
            enList:[],
            hisList:[],
            urls:'transaction_in',
            types:'in'
        }
    },
    created(){
        this.address = localStorage.getItem('address') || '';
        this.token = localStorage.getItem('token') || '';
        //  this.getdata('transaction_in');
    },
    methods:{
        changeType(index,url){
           this.isUrl=index;
           this.url=url;
           this.page=1;
           this.more="加载更多"
           if(index == 0){
               this.getdata(this.urls,this.types);
           }else if(index == 1){
               this.getHistory()
           }
           
        },
        wayChoose(index,type){
            var that=this;
            this.page=1;
            // this.type = type;
            this.more="加载更多";
            this.isChoosed=index;
            if(index == 1){
                this.urls = 'transaction_out';
                this.types = 'out';
              this.getdata(this.urls,this.types);
            }else if(index == 0){
                 this.urls = 'transaction_in';
                this.types = 'in';
                this.getdata(this.urls,this.types);
            }
            
        },
        getMore(){
            this.page = ++this.page;
            this.loading=true;
            console.log(this.page)
            console.log(this.types)
            this.getdata(this.urls,this.types);
        },
        getMore01(){
            this.page01 = ++this.page01;
            this.loading=true;
            this.getHistory()
        },
        revoke(id){
            var that=this;
            layer.confirm('确认要删除吗？',['确定','取消'],
                ()=>{
                    // var id = id;
                    that.$http({
                        url: '/api/' + 'transaction_del',
                        method:'post',
                        data:{
                            // address:that.address,
                            type:that.type,
                            id:id
                        },
                          headers: {'Authorization':  this.token}
                    }).then(res=>{
                        res = res.data;
                        console.log(res)
                        if(res.type  === 'ok'){
                            layer.msg(res.message)
                            // that.getdata();
                        }else{
                            layer.msg(res.message);
                        }
                    }).catch(error=>{
                        // console.log(error)
                    })
                },
            )
        },
        getdata(url,type){
            console.log(type)
            var page = this.page;
            this.$http({
            url: '/api/'+url,
            method:'post',
            data:{
                page:page
            },
             headers: {'Authorization':  this.token}
            }).then(res=>{
                console.log(res)
                res = res.data;
                this.loading=false;
                if(url=="transaction_in"){
                    //  page = 1;
                    //  this.enList = []
                   if(page==1){
                       console.log('-----------------')
                       this.enList = res.message.list;
                   }else{
                       var newEist = res.message.list;
                       console.log()
                       if(newEist.length<=0){
                           this.more="没有更多数据了...";
                           return;
                       }else{
                           this.enList=this.enList.concat(newEist)
                       }
                        // conaole.log(this.enList)
                   }
                     for(var i in this.enList){
                       this.enList[i].typeInfo=type;
                   } 
                   console.log(this.enList);
                }else{
                    //  page = 1;
                    //  this.enList = [];
                //    console.log(res.message.data.data);
                   if(page==1){
                      this.enList=res.message.list; 
                   }else{
                      var newEist = res.message.list; 
                       if(newEist.length<=0){
                           this.more="没有更多数据了...";
                           return;
                       }else{
                          this.enList= this.enList.concat(newEist);
                       }
                    //   console.log(this.hisList)
                   }
                   for(var i in this.newEist){
                       this.enList[i].typeInfo=type;
                   }
                }
            }).catch(error=>{
                console.log(error)
            })
        },
        //历史委托
        getHistory(){
            var page01 = this.page01
            this.$http({
            url: '/api/'+'transaction_complete',
            method:'post',
            data:{
                page:page01
            },
             headers: {'Authorization':  this.token}
            }).then(res=>{
                console.log(res);
                this.loading=false;
                 if(page01==1){
                       this.hisList = res.data.message.list
                   }else{
                       var newhist = res.data.message.list;
                       if(newhist.length<=0){
                           this.more="没有更多数据了...";
                           return;
                       }else{
                           this.hisList=this.hisList.concat(newhist)
                       }
                        // conaole.log(this.enList)
                   }
        }).catch(error=>{
                console.log(error)
            })
        }
    },
    mounted(){
        var that = this;
        that.getdata(this.urls,this.types);
        eventBus.$on('toTrade',function(){
            that.isUrl = 0;
             that.isChoosed = 0;
             that.getdata(that.urls,that.types);
        });
    }
    
}
</script>
<style scoped>
.title{height: 48px;line-height: 46px;padding: 0 40px 0 30px;}
.tab_title{display: inline-block;line-height: 46px;height: 46px;}
.tab_title span{cursor: pointer;}
.tab_title span:not(:last-child) {margin-right: 40px;}
.content{padding: 0 40px 0 30px;height: 300px;}
.list-title{line-height: 40px; border-bottom: 1px solid #ccc;height: 40px;}
.no_data{padding: 50px 0;}
.container{height: 260px;overflow: auto;}
.list-item li{line-height: 30px; color: #333;}
.list-item li span{display: inline-block;float: left; color: #333;}
.list-item li span:nth-child(3){color:#cc4951;}
.list-item li:hover{}
.list-item li span.green{color: #55a067}
</style>


