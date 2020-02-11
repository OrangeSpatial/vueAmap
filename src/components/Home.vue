<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <img src="../assets/img/orange.png" alt="" />
        <span>nCov-2019 疫情登记发布平台</span>
      </div>
      <div>
        <el-button type="primary" class="el-icon-user" circle></el-button>
          <el-tooltip content="退出登录" placement="bottom" effect="light">
            <el-button class="iconfont icon-tuichu" @click="loginout()" circle></el-button>
          </el-tooltip>
      </div>
    </el-header>
    <el-container id="content-container" direction="vertical">
      <el-aside :width="!isCollapse? '64px':'170px'">
        <div id="aTB" v-text='aDirection' @click="aToggle()"></div>
        <el-menu
        background-color="#fff"
        text-color="#000"
        active-text-color="#026bee"
        unique-opened
        :collapse-transition="false"
        router
        :default-active="activePath"
        class="el-menu-vertical-demo"
        @open="handleOpen"
        @close="handleClose"
        :collapse="!isCollapse"
        >
          <el-menu-item @click="saveIndex('/home')" index="/home">
            <i class="iconfont icon-shouye"></i>
            <span slot="title">首页</span>
          </el-menu-item>
          <el-menu-item @click="saveIndex('/patient')" index="/patient">
            <i class="iconfont icon-dusu"></i>
            <span slot="title">病人信息录入</span>
          </el-menu-item>
          <el-menu-item @click="saveIndex('/hospital')" index="/hospital">
            <i class="iconfont icon-yiyuan"></i>
            <span slot="title">医疗信息录入</span>
          </el-menu-item>
          <el-menu-item @click="saveIndex('/no-road')" index="/no-road">
            <i class="iconfont icon-icon-test5"></i>
            <span slot="title">道路封锁信息录入</span>
          </el-menu-item>
          <el-menu-item @click="saveIndex('/analyse')" index="/analyse">
            <i class="iconfont icon-online-tracking"></i>
            <span slot="title">数模分析</span>
          </el-menu-item>
        </el-menu>
        <div id="aTB" v-text='aDirection' @click="aToggle()"></div>
      </el-aside>
      <el-container direction="vertical">
        <el-main :class="{'main-hidden': isHide}">Main</el-main>
      </el-container>
      <el-footer class="footer">
          nCov-2019 疫情登记发布平台 &copy;2019 WHU
        </el-footer>
    </el-container>
  </el-container>
</template>

<script>
import AMap from 'AMap'

export default {
  data () {
    return {
      menuList: [
        { id: 1, authName: '病人信息录入' },
        { id: 2, authName: '医疗信息录入' },
        { id: 3, authName: '道路封锁信息录入' },
        { id: 4, authName: '数模分析' }
      ],
      aDirection: '《',
      isCollapse: true,
      isHide: true,
      activePath: '/home',
      key: '97d349aa7503611a2133d03f09a4a4ef'
    }
  },
  created () {
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('login')
    },
    // 切换侧边栏
    aToggle () {
      if (this.isCollapse) {
        this.aDirection = '》'
      } else {
        this.aDirection = '《'
      }
      this.isCollapse = !this.isCollapse
      // console.log(this.isCollapse)
    },
    getAMap () {
      const map = new AMap.Map('content-container', {
        mapStyle: 'amap://styles/whitesmoke', // 设置地图的显示样式
        zoom: 10, // 设置地图的缩放级别
        center: [114.30304, 30.594911]
      })
      AMap.plugin(['AMap.DistrictSearch'], function () { // 异步同时加载多个插件
        var districtSearch = new AMap.DistrictSearch({
          // 关键字对应的行政区级别，country表示国家
          level: 'country',
          //  显示下级行政区级数，1表示返回下一级行政区
          subdistrict: 1
        })
        // 搜索所有省/直辖市信息
        districtSearch.search('中国', function (status, result) {
          // 查询成功时，result即为对应的行政区信息
        })
      })
    },
    saveIndex (index) {
      window.sessionStorage.setItem('activePath', index)
      this.activePath = index
      if (this.activePath === '/home') this.isHide = true
      this.isHide = false
    }
  },
  mounted () {
    this.getAMap()
  }
}
</script>

<style lang="less" scoped>
.home-container{
  height: 100%;
  #content-container{
    background-color: aliceblue;
    z-index: 11!important;
  }
}
.el-header {
  background-color: #4827b6;
  color: #4be4ff;
  display: flex;
  justify-content: space-between;
  padding-left: 0px;
  align-items: center;
  font-size: 20px;
  div{
    display: flex;
    align-items: center;
    img {
        width: 80px;
        border-radius: 50%;
      }
      span {
        font-size: 20px;
      }
  }
}

.el-aside {
  margin-top: 16%;
  border-radius: 0 10px 10px 0;
  background-color: #94cde9;
  height: 343px;
  box-shadow: 3px 0px 2px #888;
  z-index: 10;
  .el-menu{
    border-right: none;
  }
  #aTB{
    cursor: pointer;
    height: 30px;
    color: #4827b6;
    line-height: 30px;
    text-align: center;
    background-color: #ffffff;
    border-bottom: 1px solid #00d800;
    // border-radius: 0 10px 8px 0;
  }
}

.el-main {
  color: #333;
  text-align: center;
  line-height: 160px;
  float: left;
  z-index: 10;
}
.footer{
  background-color: #f1f1f1;
  box-shadow: 0px -1px 2px #888;
  z-index: 2!important;
  text-align:center;
  color: #888;
  font-size: 14px;
  padding:4px 0;
}

.iconfont {
  padding-right: 8px;
  color: #00d8a0;
}

.main-hidden{
  visibility: hidden;
}
</style>
