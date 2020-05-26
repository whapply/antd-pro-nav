<!--
 * @Date: 2020-05-26 15:32:46
 * @Description: 顶部导航
-->

<template>
  <div>
    <ul class="nav-list">
      <li v-for="(item, index) in navList" :key="index" @click="handleNavChange(item)">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import { convertRoutes } from '@/utils/routeConvert'

export default {
  data () {
    return {
      navList: [
        {
          title: '表单',
          menuList: [],
          placeholderList: ['/dashboard', '/form']
        },
        {
          title: '页面',
          menuList: [],
          placeholderList: ['/list', '/profile', '/result', '/exception']
        },
        {
          title: 'table',
          menuList: [],
          placeholderList: ['/table']
        },
        {
          title: '个人中心',
          menuList: [],
          placeholderList: ['/account', '/other']
        },
        {
          title: '其他',
          menuList: [],
          placeholderList: ['/account', '/other']
        }
      ]
    }
  },
  computed: {
    ...mapState({
      // 动态主路由
      mainMenu: state => state.permission.addRouters
    })
  },
  created () {
    const routes = convertRoutes(this.mainMenu.find(item => item.path === '/'))
    for (let x = 0; x < routes.children.length; x++) {
      const navIdx = routes.children[x].meta.navIdx
      for (let y = 0; y < this.navList.length; y++) {
        if (navIdx === y) {
          this.navList[y].menuList.push(routes.children[x])
        }
      }
    }
  },
  methods: {
    handleNavChange (item) {
        //  更新左侧的导航
      this.$store.commit('SET_ROUTERS', item.menuList)
    }
  }
}
</script>

<style lang="less" scoped>
.nav-list {
  list-style: none;
  display: flex;
  li {
    margin: 0 20px;
  }
}
</style>
