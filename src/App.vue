<template>
  <div id="app">
    <!-- aside-menu 左侧栏 -->
    <asideMenu @triggerMarkerPopup="openPopup" ref="menu"/>
    <maskMap ref="map"/>
    <lightBox />
    <!-- 地图区块 -->
    <!--
      <div class="mask-map" id="mask-map"></div>
    -->
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import asideMenu from './components/asideMenu.vue'
import lightBox from './components/lightbox.vue'
import maskMap from './components/maskMap.vue'

export default {
  name: 'App',

  components: {
    asideMenu,
    lightBox,
    maskMap,
  },

  methods: {
    ...mapActions(['fetchLocations', 'fetchPharmacies']),
    
    openPopup(id) {
      this.$refs.map.triggerPopup(id);
    },
  },

  mounted() {
    this.fetchLocations();
    this.fetchPharmacies();
  }
}
</script>

<style lang="scss" src="./style.scss"></style>
