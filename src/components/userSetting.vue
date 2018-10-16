<template>
    <div id="user-setting" class="flex">
        <div class="setting-l">
            <div class="setting-item">
                <!-- <div class="item-title">收款方式</div> -->
                <router-link to="/userSetting">收款方式</router-link>
            </div>
        </div>
        <div class="setting-r">
            <router-view></router-view>
        </div>
    </div>
</template>

<script>
export default {};
</script>

<style lang='scss'>
#user-setting {
    min-height: 700px;
  > .setting-l {
      padding: 20px;
      line-height: 40px;
    width: 23%;
    border-right: 1px solid #ddd;
    .router-link-active{
        color:#d45858;
    }
  }
  .setting-r {
      padding: 20px;
    width: 77%;
  }
}
</style>
