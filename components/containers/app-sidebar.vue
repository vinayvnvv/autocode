<template>
  <v-navigation-drawer
    v-model="drawer"
    :mini-variant="miniVariant"
    :clipped="clipped"
    fixed
    right
    app
  >
    <div class="a-s-b">
      <div class="t">
        <v-tabs
          v-model="currentTab"
          grow
          class="app-tabs is-border-btm nav-tab"
          value="currentTab"
        >
          <v-tab key="insert">Insert</v-tab>
          <v-tab key="page">Pages</v-tab>
        </v-tabs>
      </div>
      <div v-if="currentTab == 0" class="c">
        <WidgetsList
          :ondragwidget="ondragwidget"
          :ondragwidgetend="ondragwidgetend"
        />
      </div>
      <div v-if="currentTab == 1" class="c">
        <Pages />
      </div>
    </div>
  </v-navigation-drawer>
</template>

<script>
import WidgetsList from './widgets-list'
import Pages from './pages'
export default {
  components: { WidgetsList, Pages },
  props: ['ondragwidget', 'ondragwidgetend'],
  // props: {
  //   onss: {
  //     type: 'Functio'
  //   },
  //   onDrag: {
  //     type: Function,
  //     default: () => {},
  //   },
  //   onDragEnd: {
  //     type: Function,
  //     default: () => {},
  //   },
  // },
  data() {
    return {
      clipped: true,
      drawer: true,
      fixed: true,
      currentTab: '0',
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      miniVariant: false,
      title: 'Vuetify.js',
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
.a-s-b
    display: flex
    flex-direction: column
    height: 100%
    &>.c
        flex-grow: 1
        overflow-y: auto
.nav-tab
    & .v-slide-group__wrapper
        & .v-tabs-bar__content
            & .v-tab
                text-transform: none
                font-size: 15px
                letter-spacing: -0.01em
                font-weight: 600
</style>
