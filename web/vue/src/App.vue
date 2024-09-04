<template>
  <el-container >
    <el-header height="56px">
      <app-header></app-header>
      <app-nav-menu></app-nav-menu>
    </el-header>
    <el-main >
      <div id="main-container" v-cloak>
        <keep-alive >
          <router-view v-if="$route.meta.keepAlive" />
        </keep-alive>
        <router-view v-if="!$route.meta.keepAlive" />
      </div>
    </el-main>
  </el-container>
</template>

<script>
import installService from './api/install'
import appHeader from './components/common/header.vue'
import appNavMenu from './components/common/navMenu.vue'

export default {
  name: 'App',
  data () {
    return {}
  },
  created () {
    installService.status((data) => {
      if (!data) {
        this.$router.push('/install')
      }
    })
  },
  components: {
    appHeader,
    appNavMenu
  }
}
</script>
<style>
  [v-cloak] {
    display: none !important;
  }
  body {
    margin:0;
  }
  .el-header {
    padding:0;
    margin:0;
  }
  .el-container {
    padding:0;
    margin:0;
    width: 100%;
  }
  .el-main {
    padding:0;
    margin:0;
  }
  #main-container .el-main {
    height: calc(100vh - 56px);
    padding:20px 0 20px 20px;
  }
  .el-aside .el-menu {
    height: 100%;
  }
  .el-table {
    width: 100%;
    margin-top: 10px;
  }
</style>
