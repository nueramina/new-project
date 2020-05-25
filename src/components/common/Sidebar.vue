<template>
  <div class="sidebar">
    <el-menu
      class="sidebar-el-menu"
      :default-active="onRoutes"
      :collapse="collapse"
      background-color="#324157"
      text-color="#bfcbd9"
      active-text-color="#20a0ff"
      unique-opened
      router
    >
      <template v-for="item in items">
        <template v-if="item.subs">
          <el-submenu :index="item.index" :key="item.index">
            <template slot="title">
              <i :class="item.icon"></i>
              <span slot="title">{{ item.title }}</span>
            </template>
            <template v-for="subItem in item.subs">
              <el-submenu v-if="subItem.subs" :index="subItem.index" :key="subItem.index">
                <template slot="title">{{ subItem.title }}</template>
                <el-menu-item
                  v-for="(threeItem,i) in subItem.subs"
                  :key="i"
                  :index="threeItem.index"
                >{{ threeItem.title }}</el-menu-item>
              </el-submenu>
              <el-menu-item v-else :index="subItem.index" :key="subItem.index">{{ subItem.title }}</el-menu-item>
            </template>
          </el-submenu>
        </template>
        <template v-else>
          <el-menu-item :index="item.index" :key="item.index">
            <i :class="item.icon"></i>
            <span slot="title">{{ item.title }}</span>
          </el-menu-item>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>
import bus from '../common/bus';
export default {
    data() {
        return {
            collapse: false,
            items: [
                // {
                //     icon: 'el-icon-lx-home',
                //     index: 'dashboard',
                //     title: '系统首页'
                // },
                // {
                //     icon: 'el-icon-lx-cascades',
                //     index: 'table',
                //     title: '基础表格'
                // },
                // {
                //     icon: 'el-icon-lx-copy',
                //     index: 'tabs',
                //     title: 'tab选项卡'
                // },
                {
                    icon: 'el-icon-lx-calendar',
                    index: '1',
                    title: '录入',
                    subs: [
                        {
                            index: 'dashboard',
                            title: '首次录入'
                        },
                        {
                            index: 'table',
                            title: '重启录入'
                        },
                        {
                            icon: 'el-icon-lx-cascades',
                            index: 'table',
                            title: '整列'
                        },
                        {
                            index: 'form',
                            title: '诸量计算'
                        },
                        {
                            index: 'charts',
                            title: '作图'
                        }
                    ]
                },
                // {
                //     icon: 'el-icon-lx-emoji',
                //     index: 'icon',
                //     title: '自定义图标'
                // },
                // {
                //     icon: 'el-icon-pie-chart',
                //     index: 'charts',
                //     title: 'schart图表'
                // },
                {
                    icon: 'el-icon-rank',
                    index: '2',
                    title: '变化计算',
                    subs: [
                        {
                            index: 'drag',
                            title: '变化计算'
                        }
                        // {
                        //     index: 'dialog',
                        //     title: '拖拽弹框'
                        // }
                    ]
                },
                // {
                //     icon: 'el-icon-lx-global',
                //     index: 'i18n',
                //     title: '国际化功能'
                // },
                {
                    icon: 'el-icon-lx-warn',
                    index: '3',
                    title: '分类',
                    subs: [
                        {
                            index: 'dialog',
                            title: '分类录入'
                        },
                        {
                            index: 'icon',
                            title: '分类整列'
                        },
                        {
                            index: '404',
                            title: '分类计算'
                        }
                        // {
                        //     index: '404',
                        //     title: '404页面'
                        // }
                    ]
                },
            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
    }
};
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}
</style>
