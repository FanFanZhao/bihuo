<template>
    <div class="wrap">
        <ul class="ul">
            <li class="flex alcenter list_title ft14">
                <span class="flex1 tc">账号</span>
                <span class="flex1 tc">币种交易符号</span>
                <span class="flex1 tc">币种中文名</span>
                <span class="flex1 tc">币种总量</span>
                 <span class="flex1 tc">操作</span>
            </li>
            <li v-for="(item,index) in currency_list" :key="index" class="flex alcenter list_item ft12">
                <span class="flex1 tc">{{item.email}}</span>
                <span class="flex1 tc">{{item.token}}</span>
                <span class="flex1 tc">{{item.chinese_name}}</span>
                <span class="flex1 tc">{{item.sum}}</span>
                 <span class="flex1 tc red" @click="edit(item.id)">编辑</span>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
     data(){
         return{
            currency_list:[]
         }
     },
     created(){
        this.token = window.localStorage.getItem("token") || "";
        if(this.token == ''){
        this.$router.push('/components/login');
    }
     },
     mounted(){
        this.init();
     },
     methods:{
         init(){
             this.$http({
                  url: "/api/currency/request_add_currency_list",
                  method: "get",
                  headers: { Authorization: this.token },

             }).then(res =>{
                  console.log(res);
                  if(res.data.type == 'ok'){
                      this.currency_list = res.data.message.data;
                  }
             })
         },
         edit(id){
             this.$router.push({name:'currencyEdit',params:{id:id}})
            
         }
     }
}
</script>
<style scoped>
    .wrap{
        width: 1200px;
        margin: 50px auto;
        min-height: 700px;
    }
    .ul li{
        padding: 20px 0;
        border-bottom: 1px solid #eee;
    }
    .list_title{
        background: #eee;
    }
    .list_item:hover{
        background: #eee;
    }
    .red:hover{
        cursor: pointer;
    }
</style>
