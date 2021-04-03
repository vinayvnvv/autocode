<template>
  <v-menu
    v-model="menu"
    content-class="widget-menu-bar"
    :close-on-content-click="false"
    offset-y="40"
    nudge-width="'auto'"
  >
    <template v-slot:activator="{ on, attrs }">
      <div class="image-view" v-bind="attrs" v-on="on">
        <input :id="id" type="file" class="upld" @change="onImageChange" />
        <v-template v-if="data.image">
          <img :src="data.image" class="img" :style="getStyles(data)" />
        </v-template>
        <v-template v-if="!data.image">
          <label v-ripple :for="id" class="empty-space">
            <v-icon>mdi-upload</v-icon>
            <span>Upload Image</span>
          </label>
        </v-template>
      </div>
    </template>
    <v-toolbar v-if="data.image" flat height="48px" floating class="w-toolbar">
      <label v-ripple :for="id" class="upload-lbl btn itm">
        Change Image
      </label>
      <v-select
        v-model="data.borderType"
        :items="items"
        label="Border Type"
        class="itm"
        style="width: 200px"
        hide-details
        item-text="label"
        item-value="value"
        dense
        solo
      ></v-select>
      <v-btn icon small class="itm">
        <v-icon small>mdi-delete</v-icon>
      </v-btn>
    </v-toolbar>
    <!-- <v-sheet class="d-flex" color="grey lighten-3" elevation="0">
      <v-btn text fab small>
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-sheet> -->
  </v-menu>
</template>

<script>
export const IMG_VIEW_INITIAL_DATA = {
  type: 'text',
  componentName: 'ImageView',
  data: {
    imageURL: 'Vinay',
  },
}
export default {
  data() {
    return {
      data: {
        image: '',
        borderType: 'squared',
      },
      items: [
        { label: 'Squared', value: 'squared' },
        { label: 'Circle', value: 'circle' },
        { label: 'Semi Circle', value: 'semi-circle' },
      ],
      id: new Date().toISOString(),
    }
  },
  methods: {
    onInput(e) {
      console.log('dsdsd', e)
    },
    onImageChange(e) {
      const {
        target: { files },
      } = e
      if (files && files[0]) {
        this.data.image = URL.createObjectURL(files[0])
      }
      console.log('image chnage', files)
    },
    getStyles(data) {
      const style = {}
      if (data.borderType) {
        let border = 0
        if (data.borderType === 'semi-circle') border = '11px'
        if (data.borderType === 'circle') border = '100%'
        style.borderRadius = border
      }
      return style
    },
  },
}
</script>

<style lang="sass">
.image-view
  height: 100%
  & img
    width: 100%
    height: 100%
  & .upld
    width: 0
    height: 0
    opacity: 0
    position: absolute
  & .empty-space
    width: 100%
    height: 100%
    display: flex
    align-items: center
    justify-content: center
    flex-direction: column
    &>span
      margin-top: 4px
</style>
