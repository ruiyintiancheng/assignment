<template>
  <div class="app-wrapper" >
    <div class="main-container" >
      <sidebar class="sidebar-container"  :style="{height:(height)+'px',backgroundColor:'#3164b7'}"></sidebar>
      <app-main class="app-main" ref="appMain" :style="{height:height+'px',backgroundColor:conColor}">></app-main>
    </div>
  </div>
</template>

<script>
import { Sidebar, AppMain } from './components'

export default {
  name: 'layout',
  components: {
    AppMain,
    Sidebar
  },
  computed: {
    conColor: function() {
      if (this.$route.name === 'learn') {
        return '#def5fb'
      } else {
        return 'none'
      }
    }
  },
  created() {
    this.setCurrentHeight()
    this.$i18n.locale = this.$store.state.app.language
    window.addEventListener('resize', function() {
      this.setCurrentHeight()
    }.bind(this), false)
  },
  data() {
    return {
      height: null
    }
  },
  methods: {
    setCurrentHeight() {
      this.height = document.body.offsetHeight
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .main-container{
    overflow: auto;
    overflow-x: hidden;
    width: 100%;
  }
  .app-wrapper {
   // @include clearfix;
    position: relative;
    height: 100%;
    width: 100%;
    overflow: hidden;
  }
  .drawer-bg {
    background: #000;
    opacity: 0.3;
    width: 100%;
    top: 0;
    height: 100%;
    position: absolute;
    z-index: 999;
  }
</style>
