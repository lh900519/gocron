<template>
  <el-container>
    <user-sidebar></user-sidebar>
    <el-main>
      <el-breadcrumb separator-class="el-icon-arrow-right" style="margin-bottom:20px">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item :to="{ path: '/user' }">用户管理</el-breadcrumb-item>
          <el-breadcrumb-item>编辑</el-breadcrumb-item>
      </el-breadcrumb>
      <el-form ref="form" class="page-form" :model="form" :rules="formRules" label-width="100px">
        <el-row>
          <el-col :span="8">
            <el-input v-model="form.id" type="hidden"></el-input>
            <el-form-item label="用户名" prop="name">
              <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="邮箱" prop="email">
              <el-input v-model="form.email"></el-input>
            </el-form-item>
            <template v-if="!form.id">
              <el-form-item label="密码" prop="password">
                <el-input v-model="form.password" type="password"></el-input>
              </el-form-item>
              <el-form-item label="确认密码" prop="confirm_password">
                <el-input v-model="form.confirm_password" type="password"></el-input>
              </el-form-item>
            </template>
            <el-form-item label="角色" prop="is_admin">
              <el-radio-group v-model="form.is_admin">
                <el-radio :label="0">
                  普通用户
                  <el-tooltip placement="top">
                    <div slot="content">
                      权限简要:<br/><br/>
                      - 查看所有任务和日志
                    </div>
                    <i class="el-icon-question"></i>
                  </el-tooltip>
                </el-radio>
                <el-radio :label="1">
                  管理员
                  <el-tooltip placement="top">
                    <div slot="content">
                      权限简要:<br/><br/>
                      - 查看所有任务和日志<br/>
                      - 管理自己的任务
                    </div>
                    <i class="el-icon-question"></i>
                  </el-tooltip>
                </el-radio>
                <el-radio :label="2">
                  超级管理员
                  <el-tooltip placement="top">
                    <div slot="content">
                      权限简要:<br/><br/>
                      - 管理所有任务和日志<br/>
                      - 用户管理<br/>
                      - 系统管理
                    </div>
                    <i class="el-icon-question"></i>
                  </el-tooltip>
                </el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="状态" prop="status">
              <el-radio-group v-model="form.status">
                <el-radio :label="1">启用</el-radio>
                <el-radio :label="0">禁用</el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submit()">保存</el-button>
              <el-button @click="cancel">取消</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </el-main>
  </el-container>
</template>

<script>
import userSidebar from './sidebar'
import userService from '../../api/user'
export default {
  name: 'user-edit',
  data: function () {
    return {
      form: {
        id: '',
        name: '',
        email: '',
        is_admin: 0,
        password: '',
        confirm_password: '',
        status: 1
      },
      formRules: {
        name: [
          {required: true, message: '请输入用户名', trigger: 'blur'}
        ],
        email: [
          {type: 'email', required: true, message: '请输入有效邮箱地址', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'}
        ],
        confirm_password: [
          {required: true, message: '请再次输入密码', trigger: 'blur'}
        ]
      }
    }
  },
  components: {userSidebar},
  created () {
    const id = this.$route.params.id
    if (!id) {
      return
    }
    userService.detail(id, (data) => {
      if (!data) {
        this.$message.error('数据不存在')
        return
      }
      this.form.id = data.id
      this.form.name = data.name
      this.form.email = data.email
      this.form.is_admin = data.is_admin
      this.form.status = data.status
    })
  },
  methods: {
    submit () {
      this.$refs['form'].validate((valid) => {
        if (!valid) {
          return false
        }
        this.save()
      })
    },
    save () {
      userService.update(this.form, () => {
        this.$router.push('/user')
      })
    },
    cancel () {
      this.$router.push('/user')
    }
  }
}
</script>
