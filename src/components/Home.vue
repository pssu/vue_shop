<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/logo.png" alt />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区 -->
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409BFF"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
            <!-- 一级菜单的模板区 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{ item.authName }}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/' + subItem.path)"
            >
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧主体内容 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      // 模拟后端数据
      menuList: [
        {
          id: 1,
          authName: '用户管理',
          children: [
            {
              id: 10,
              authName: '用户列表',
              path: 'users'
            }
          ]
        },
        {
          id: 2,
          authName: '权限管理',
          children: [
            {
              id: 20,
              authName: '角色列表',
              path: 'roles'
            },
            {
              id: 21,
              authName: '权限列表',
              path: 'rights'
            }
          ]
        },
        {
          id: 3,
          authName: '商品管理',
          children: [
            {
              id: 30,
              authName: '商品列表',
              path: 'goods'
            },
            {
              id: 31,
              authName: '分类参数',
              path: ''
            },
            {
              id: 32,
              authName: '商品分类',
              path: 'category'
            }
          ]
        },
        {
          id: 4,
          authName: '订单管理',
          children: [
            {
              id: 41,
              authName: '订单列表',
              path: ''
            }
          ]
        },
        {
          id: 5,
          authName: '数据统计'
        }
      ],
      iconsObj: {
        '1': 'el-icon-user-solid',
        '2': 'el-icon-s-cooperation',
        '3': 'el-icon-s-goods',
        '4': 'el-icon-s-order',
        '5': 'el-icon-data-analysis'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  methods: {
    logout() {
      // 清空token
      window.sessionStorage.clear()
      // 跳转到登录页面
      this.$router.push('/login')
    },
    // 点击按钮，实现菜单的折叠和打开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    // 点击二级菜单的时候把路径放到sessionStorage中
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  },
  created() {
    //   从后端获取菜单列表
    //   this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}

.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  color: #fff;
  padding-left: 0;
  align-items: center;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}

.el-main {
  background-color: #eaedf1;
}

.toggle-button {
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
