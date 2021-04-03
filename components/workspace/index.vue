<template>
  <div id="workspace" class="workspace-c">
    <grid-layout
      id="workspaceArea"
      :layout.sync="layout"
      :col-num="12"
      :row-height="30"
      :is-draggable="true"
      :is-resizable="true"
      :is-mirrored="false"
      :vertical-compact="true"
      :margin="[10, 10]"
      :use-css-transforms="true"
      @layout-updated="layoutUpdated"
    >
      <grid-item
        v-for="item in layout"
        :key="item.i"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
        :i="item.i"
      >
        {{ item.i }}
      </grid-item>
    </grid-layout>
  </div>
</template>

<script>
import VueGridLayout from 'vue-grid-layout'
export default {
  components: {
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem,
  },
  data() {
    return {
      layout: [
        { x: 0, y: 0, w: 2, h: 2, i: '0' },
        { x: 2, y: 0, w: 2, h: 4, i: '1' },
        { x: 4, y: 0, w: 2, h: 5, i: '2' },
      ],
    }
  },
  methods: {
    layoutUpdated(newLayout) {
      console.log('new layout', newLayout)
      this.$store.commit('updateState', newLayout)
    },
  },
}
</script>

<style lang="sass">
.workspace-c
    background-color: #fff
</style>
