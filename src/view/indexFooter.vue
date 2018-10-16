<template>
    <div id="footer">
        <div>
            <span>联系我们</span>
            <span>费率标准</span>
            <span>API文档</span>
            <span>用户协议</span>
            <span>本站公告</span>
        </div>
        <p class="">© 2013-2018 环球数贸信息平台</p>
    </div>
</template>

<script>
export default {

}
</script>

<style lang='scss'>
#footer{
    background: #09162e;
    color:#aabdbc;
    font-size: 14px;
    text-align: center;
    >div{
        padding: 20px;
        span{
            margin-right: 30px;cursor: pointer;
            &:hover{color:#d45858}
        }
    }
    p{
        padding: 10px;
    }
}
</style>
