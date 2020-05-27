<!--
 * @Date: 2020-05-26 15:32:46
 * @Description: 顶部导航
-->

<template>
  <div>
    <ul class="nav-list">
      <li
        v-for="(item, index) in navList"
        :key="index"
        @click="handleNavChange(item, index)"
        :class="index === actIdx ? 'active' : ''"
      >{{ item.title }}</li>
    </ul>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import { convertRoutes } from '@/utils/routeConvert'

export default {
  data () {
    return {
      actIdx: 0,
      navList: [
        {
          title: '表单',
          menuList: []
        },
        {
          title: '页面',
          menuList: []
        },
        {
          title: 'table',
          menuList: []
        },
        {
          title: '个人中心',
          menuList: []
        },
        {
          title: '其他',
          menuList: []
        }
      ]
    }
  },
  computed: {
    ...mapState({
      // 动态主路由
      mainMenu: state => state.permission.initAddRouters
    })
  },
  created () {
    const routes = convertRoutes(this.mainMenu.find(item => item.path === '/'))
    if (routes) {
      for (let x = 0; x < routes.children.length; x++) {
        const navIdx = routes.children[x].meta.navIdx
        for (let y = 0; y < this.navList.length; y++) {
          if (navIdx === y) {
            this.navList[y].menuList.push(routes.children[x])
          }
        }
      }
      this.initSelectNav()
    }
  },
  methods: {
    handleNavChange (item, index) {
      //  更新左侧的导航
      this.actIdx = index
      this.$store.commit('SET_ROUTERS', item.menuList)
    },
    initSelectNav () {
      const locationHash = location.pathname
      const parentPath = locationHash.split('/').filter(Boolean)[0]
      for (let y = 0; y < this.navList.length; y++) {
        const menuSideList = this.navList[y].menuList
        const menuSideList2 = menuSideList.map(item => item.path)
        if (menuSideList2.includes('/' + parentPath)) {
          //  高亮顶部菜单
          this.actIdx = y
          //  初始化 选中左边菜单
          this.$store.commit('SET_ROUTERS', this.navList[y].menuList)
        }
      }
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
    &.active {
      color: rgb(18, 150, 219);
    }
  }
}
</style>
