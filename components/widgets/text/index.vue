<template>
  <v-menu
    v-model="menu"
    :close-on-content-click="true"
    offset-y="40"
    nudge-width="'auto'"
  >
    <template v-slot:activator="{ on, attrs }">
      <div class="text-view" v-bind="attrs" v-on="on">
        <ckeditor
          v-model="text"
          :editor="editor"
          :config="editorConfig"
          placeholder="sdasdsa"
          @input="onInput"
        />
        <span v-if="text.length === 0" class="ck-placeholder">Enter Text</span>
      </div>
    </template>
    <v-sheet class="d-flex" color="grey lighten-3" elevation="0">
      <v-btn text fab small>
        <v-icon>mdi-delete</v-icon>
      </v-btn>
    </v-sheet>
  </v-menu>
</template>

<script>
import InlineEditor from '@ckeditor/ckeditor5-build-inline'
import CKEditor from '@ckeditor/ckeditor5-vue2'
export const TEXT_VIEW_INITIAL_DATA = {
  type: 'text',
  componentName: 'TextView',
  data: {
    text: 'Vinay',
  },
}
export default {
  components: {
    ckeditor: CKEditor.component,
  },
  data() {
    return {
      text: '',
      editor: InlineEditor,
      editorConfig: {
        alignment: {
          options: ['left', 'right', 'center'],
        },
        toolbar: [
          'heading',
          '|',
          'bold',
          'alignment',
          'italic',
          'link',
          'bulletedList',
          'numberedList',
          'blockQuote',
          'undo',
          'redo',
        ],
      },
    }
  },
  methods: {
    onInput(e) {
      console.log('dsdsd', e)
    },
  },
}
</script>

<style lang="sass">
.text-view
  height: 100%
  cursor: text
  & .ck-placeholder
    position: absolute
    top: 11px
    left: 11px
    color: #bbb
  & .ck
    min-height: 100%
    &.ck-focused
      border: none !important
</style>
