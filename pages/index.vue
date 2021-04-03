<template>
  <v-app>
    <AppSideBar
      :layout="layout"
      :ondragwidget="drag"
      :ondragwidgetend="dragend"
    />
    <AppToolBar />
    <v-main class="app-main">
      <v-container class="app-main-c">
        <div id="workspace" class="workspace-c">
          <grid-layout
            id="workspaceArea"
            ref="gridlayout"
            :layout.sync="layout"
            :col-num="12"
            :row-height="30"
            :is-draggable="true"
            :is-resizable="true"
            :is-mirrored="false"
            :vertical-compact="true"
            :margin="[10, 10]"
            :use-css-transforms="true"
          >
            <grid-item
              v-for="item in layout"
              :key="item.i"
              :x="item.x"
              :y="item.y"
              :w="item.w"
              :h="item.h"
              :i="item.i"
              drag-allow-from=".draggable-handle"
              drag-ignore-from=".no-drag"
            >
              <div v-if="item.widget" class="grid-itm">
                <div class="draggable-handle">
                  <v-icon>mdi-drag</v-icon>
                </div>
                <component :is="item.widget.componentName" />
              </div>
            </grid-item>
          </grid-layout>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import VueGridLayout from 'vue-grid-layout'
import AppToolBar from './../components/containers/app-toolbar'
import AppSideBar from './../components/containers/app-sidebar'
import Widgets from './../components/widgets/index'
// import Text from './../components/widgets/text/text'

const mouseXY = { x: null, y: null }
const DragPos = { x: null, y: null, w: 1, h: 1, i: null }
const virtualDragBox = { w: 2, h: 2 }

export default {
  components: {
    AppToolBar,
    AppSideBar,
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem,
    ...Widgets,
    // Text,
  },
  data() {
    return {
      layout: [],
    }
  },
  mounted() {
    document.addEventListener(
      'dragover',
      function (e) {
        mouseXY.x = e.clientX
        mouseXY.y = e.clientY
      },
      false
    )
  },
  methods: {
    drag(e) {
      const parentRect = document
        .getElementById('workspace')
        .getBoundingClientRect()
      let mouseInGrid = false
      if (
        mouseXY.x > parentRect.left &&
        mouseXY.x < parentRect.right &&
        mouseXY.y > parentRect.top &&
        mouseXY.y < parentRect.bottom
      ) {
        mouseInGrid = true
      }
      if (
        mouseInGrid === true &&
        this.layout.findIndex((item) => item.i === 'drop') === -1
      ) {
        this.layout.push({
          x: (this.layout.length * 2) % (this.colNum || 12),
          y: this.layout.length + (this.colNum || 12), // puts it at the bottom
          w: 1,
          h: 1,
          i: 'drop',
        })
      }
      const index = this.layout.findIndex((item) => item.i === 'drop')
      if (index !== -1) {
        try {
          this.$refs.gridlayout.$children[
            this.layout.length
          ].$refs.item.style.display = 'none'
        } catch {}
        const el = this.$refs.gridlayout.$children[index]
        el.dragging = {
          top: mouseXY.y - parentRect.top,
          left: mouseXY.x - parentRect.left,
        }
        const newPos = el.calcXY(
          mouseXY.y - parentRect.top,
          mouseXY.x - parentRect.left
        )
        if (mouseInGrid === true) {
          this.$refs.gridlayout.dragEvent(
            'dragstart',
            'drop',
            newPos.x,
            newPos.y,
            virtualDragBox.w,
            virtualDragBox.h
          )
          DragPos.i = String(index)
          DragPos.x = this.layout[index].x
          DragPos.y = this.layout[index].y
        }
        if (mouseInGrid === false) {
          this.$refs.gridlayout.dragEvent(
            'dragend',
            'drop',
            newPos.x,
            newPos.y,
            virtualDragBox.w,
            virtualDragBox.h
          )
          this.layout = this.layout.filter((obj) => obj.i !== 'drop')
        }
      }
    },
    dragend(widget) {
      console.log('on add widget', widget)
      const parentRect = document
        .getElementById('workspace')
        .getBoundingClientRect()
      let mouseInGrid = false
      if (
        mouseXY.x > parentRect.left &&
        mouseXY.x < parentRect.right &&
        mouseXY.y > parentRect.top &&
        mouseXY.y < parentRect.bottom
      ) {
        mouseInGrid = true
      }
      if (mouseInGrid === true) {
        this.$refs.gridlayout.dragEvent(
          'dragend',
          'drop',
          DragPos.x,
          DragPos.y,
          5,
          2
        )
        this.layout = this.layout.filter((obj) => obj.i !== 'drop')
        // UNCOMMENT below if you want to add a grid-item
        this.layout.push({
          x: DragPos.x,
          y: DragPos.y,
          w: virtualDragBox.w,
          h: virtualDragBox.h,
          i: DragPos.i,
          widget: widget.initData,
        })
      }
    },
  },
}
</script>

<style lang="sass">
.app-main
  background-color: #f0f0f099
  .app-main-c
    min-height: 100%
  .workspace-c
    background-color: #fff
    box-shadow: 0 0 10px -5px #676767
    min-height: 80vh
  & .grid-itm
    position: absolute
    top: 0
    left: 0
    height: 100%
    width: 100%
    border: 1px dotted #d9d9d999
    & .draggable-handle
      display: none
      position: absolute
      height: 30px
      width: 30px
      left: -32px
      border-radius: 0 0 5px 0
      background-color: #f9f9f9
    &:hover
      box-shadow: 0 0 0 1px blue
      border-color: transparent
      & .draggable-handle
        display: flex
        align-items: center
        justify-content: center
</style>
