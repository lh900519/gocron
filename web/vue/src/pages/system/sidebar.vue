<template>
  <el-aside width="150px">
    <el-menu
      :default-active="currentRoute"
      mode="vertical"
      background-color="#334157"
      text-color="#fff"
      active-text-color="#ffd04b"
      router>
      <el-menu-item index="/system">
        <i class="el-icon-bell"></i>
        <span slot="title">通知配置</span>
      </el-menu-item>
      <el-menu-item index="/system/login-log">
        <i class="el-icon-document"></i>
        <span slot="title">登录日志</span>
      </el-menu-item>
    </el-menu>
  </el-aside>
</template>

<script>
export default {
  name: 'system-sidebar',
  data () {
    return {}
  },
  computed: {
    currentRoute () {
      if (this.$route.path === '/system/login-log') {
        return '/system/login-log'
      }
      return '/system'
    }
  }
}
</script>
