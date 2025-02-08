<template>
  <!--  主体背景色设置-->
  <div :class="classObj" class="app-wrapper" :style="{'--current-color': theme}">
    <div v-if="device==='mobile'&&sidebar.opened" class="drawer-bg" @click="handleClickOutside"/>
    <sidebar v-if="!sidebar.hide" class="sidebar-container"/>
    <div :class="{hasTagsView:needTagsView,sidebarHide:sidebar.hide}" class="main-container">
      <div :class="{'fixed-header':fixedHeader}" style="padding: 20px 0px">
        <navbar/>
        <!--        取消tags标签-->
        <!--        <tags-view v-if="needTagsView"/>-->
      </div>
      <div style="padding-right: 10px">
        <app-main/>
      </div>
      <right-panel>
        <settings/>
      </right-panel>
    </div>
  </div>
</template>

<script>
import RightPanel from '@/components/RightPanel'
import {AppMain, Navbar, Settings, Sidebar, TagsView} from './components'
import ResizeMixin from './mixin/ResizeHandler'
import {mapState} from 'vuex'
import variables from '@/assets/styles/variables.scss'

export default {
  name: 'Layout',
  components: {
    AppMain,
    Navbar,
    RightPanel,
    Settings,
    Sidebar,
    TagsView
  },
  mixins: [ResizeMixin],
  computed: {
    ...mapState({
      theme: state => state.settings.theme,
      sideTheme: state => state.settings.sideTheme,
      sidebar: state => state.app.sidebar,
      device: state => state.app.device,
      needTagsView: state => state.settings.tagsView,
      fixedHeader: state => state.settings.fixedHeader
    }),
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
        withoutAnimation: this.sidebar.withoutAnimation,
        mobile: this.device === 'mobile'
      }
    },
    variables() {
      return variables;
    }
  },
  methods: {
    handleClickOutside() {
      this.$store.dispatch('app/closeSideBar', {withoutAnimation: false})
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~@/assets/styles/mixin.scss";
@import "~@/assets/styles/variables.scss";

.app-wrapper {
  @include clearfix;
  position: relative;
  height: 100%;
  width: 100%;
  //background: white;

  &.mobile.openSidebar {
    position: fixed;
    top: 0;
  }
}

//模糊背景色块
.app-wrapper::after {
  position: absolute;
  width: 400px; /* 模糊区域的宽度 */
  height: 200px; /* 模糊区域的高度 */
  //background-color: rgba(168, 132, 224, 0.4); /* 淡紫色 */
  background: rgba(208, 255, 0, 0.15); /* 淡黄色 */
  //border-radius: 50%;
  filter: blur(100px); /* 高斯模糊效果 */
  z-index: -1; /* 确保模糊效果在页面内容下方 */
  top: -100px;
  left: -50px;
}

//.app-wrapper::before {
//  content: "";
//  position: absolute;
//  width: 400px; /* 模糊区域的宽度 */
//  height: 200px; /* 模糊区域的高度 */
//  background-color: rgba(211, 176, 0, 0.2); /* 淡紫色 */
//  //border-radius: 50%;
//  filter: blur(100px); /* 高斯模糊效果 */
//  z-index: -1; /* 确保模糊效果在页面内容下方 */
//  top: 100px;
//  left: 50px;
//}

.drawer-bg {
  background: #000;
  opacity: 0.3;
  width: 100%;
  top: 0;
  height: 100%;
  position: absolute;
  z-index: 999;
}

.fixed-header {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 9;
  width: calc(100% - #{$base-sidebar-width});
  transition: width 0.28s;
}

.hideSidebar .fixed-header {
  width: calc(100% - 54px);
}

.sidebarHide .fixed-header {
  width: 100%;
}

.mobile .fixed-header {
  width: 100%;
}
</style>
