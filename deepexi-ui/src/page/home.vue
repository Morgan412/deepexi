<template>
  <div class="app">
    <el-container>
      <el-aside class="app-side app-side-left" :class="isCollapse ? 'app-side-collapsed' : 'app-side-expanded'">
        <div class="app-side-logo">
          <!-- <img src="@/assets/logo.png" :width="isCollapse ? '60' : '60'" height="60" /> -->
          <div id="logo" :class="isCollapse ? 'logo-collapse-width' : 'logo-width'" style="text-align: center;line-height: 60px;">
            deepexi
          </div>
        </div>
        <div>
          <!-- 菜单 -->
          <el-menu :default-active="$route.path" router class="el-menu-vertical-demo" :collapse="isCollapse">
            <template v-for="route in $router.options.routes" v-if="route.children && route.children.length">
              <template v-for="item in route.children">
                <el-menu-item :key="route.path + '/' + item.path" :index="item.path">
                  <i :class="item.icon"></i>
                  <span slot="title">{{ item.name }}</span>
                </el-menu-item>
              </template>
            </template>
          </el-menu>
        </div>
      </el-aside>

      <el-container>
        <el-header class="app-header">
          <div style="width: 60px; cursor: pointer;" @click.prevent="toggleSideBar">
            <i v-show="!isCollapse" class="el-icon-d-arrow-left"></i>
            <i v-show="isCollapse" class="el-icon-d-arrow-right"></i>
          </div>

          <!-- <el-menu default-active="/" router class="el-menu-demo tab-page" mode="horizontal" @select="handleSelect"
            active-text-color="#409EFF">
            <el-menu-item index="/">首页</el-menu-item>
          </el-menu> -->

          <div class="app-header-userinfo">
            <el-dropdown trigger="hover" :hide-on-click="false">
              <span class="el-dropdown-link">
                {{ username }}
                <i class="el-icon-arrow-down el-icon--right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>我的消息</el-dropdown-item>
                <el-dropdown-item>设置</el-dropdown-item>
                <el-dropdown-item divided @click.native="logout">退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
        </el-header>

        <el-main class="app-body">
          <!-- <template>
            <router-view/>
          </template> -->
          <transition name="fade" mode="out-in">
            <router-view></router-view>
          </transition>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
  export default {
    name: 'Container',
    data() {
      return {
        username: '',
        isCollapse: false
      }
    },
    methods: {
      toggleSideBar() {
        this.isCollapse = !this.isCollapse
      },
      logout: function() {
        this.$confirm('确认退出?', '提示', {})
          .then(() => {
            sessionStorage.removeItem('user');
            this.$router.push('/login');
          })
          .catch(() => {});
      }
    },
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    mounted: function() {
      let user = sessionStorage.getItem('user');
      if (user) {
        this.username = user;
      }
    },
  }
</script>

<style>
</style>
