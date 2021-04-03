<template>
  <div class="widgets-list">
    <v-expansion-panels
      v-model="panel"
      tile
      flat
      multiple
      accordion
      class="panel"
    >
      <v-expansion-panel>
        <v-expansion-panel-header :ripple="{ class: 'ripple-bg' }"
          >Basic</v-expansion-panel-header
        >
        <v-expansion-panel-content>
          <div class="basic-c">
            <div
              v-for="itm in widgets.basic"
              :key="itm.type"
              draggable="true"
              unselectable="on"
              class="b-c-itm"
              @drag="ondragwidget"
              @dragend="ondragwidgetend(itm)"
            >
              <v-icon class="b-c-i" :color="itm.color">{{ itm.icon }}</v-icon>
              <span class="b-c-t">{{ itm.name }}</span>
            </div>
          </div>
        </v-expansion-panel-content>
      </v-expansion-panel>
      <v-expansion-panel>
        <v-expansion-panel-header :ripple="{ class: 'ripple-bg' }">
          Layouts
        </v-expansion-panel-header>
        <v-expansion-panel-content> Basics Layouts </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </div>
</template>

<script>
import { WIDGETS_INIT_DATA } from './../widgets'
export default {
  props: ['ondragwidget', 'ondragwidgetend'],
  data() {
    return {
      panel: [0, 1, 2, 3, 4, 5],
      widgets: {
        basic: [
          {
            name: 'Text',
            type: 'text',
            icon: 'mdi-text-recognition',
            color: '#F44336',
            initData: WIDGETS_INIT_DATA.TEXT_VIEW,
          },
          {
            name: 'Image',
            type: 'image',
            icon: 'mdi-image-outline',
            color: '#3F51B5',
            initData: WIDGETS_INIT_DATA.IMAGE_VIEW,
          },
          {
            name: 'Button',
            type: 'button',
            icon: 'mdi-gesture-tap-button',
            color: '#03A9F4',
          },
          {
            name: 'Divider',
            type: 'divider',
            icon: 'mdi-drag-horizontal-variant',
            color: '#4CAF50',
          },
        ],
      },
    }
  },
  methods: {
    onWidgetDrag(e) {
      console.log('drag wifet')
      this.$emit('onDrag', e)
    },
    onWidgetDragEnd(e) {
      this.$emit('onDragEnd', e)
    },
  },
}
</script>

<style lang="sass">
.widgets-list
  & .panel
      z-index: initial !important
  & .basic-c
    display: flex
    align-items: center
    flex-wrap: wrap
    & .b-c-itm
      width: 50%
      height: 70px
      display: flex
      flex-direction: column
      position: relative !important
      justify-content: center
      border-radius: 3px
      align-items: center
      cursor: pointer
      overflow: hidden
      transition: 0.23s background-color
      &:hover
        background-color: #f9f9f9
      &:active
        background-color: #e9e9e9
      // &>.b-c-i
      &>.b-c-t
        margin-top: 7px
        font-size: 13px
        font-weight: 600
        letter-spacing: -0.01em
</style>
