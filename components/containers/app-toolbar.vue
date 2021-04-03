<template>
  <v-app-bar
    :clipped-right="clipped"
    fixed
    class="app-bar"
    elevation
    app
    outlined
    light
    tile
    color="white"
  >
    <v-toolbar-title class="title" v-text="title" />
    <v-spacer />
    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          icon
          class="mr-2"
          v-bind="attrs"
          @click.stop="rightDrawer = !rightDrawer"
          v-on="on"
        >
          <v-icon size="21">mdi-desktop-mac</v-icon>
        </v-btn>
      </template>
      <span>Preview</span>
    </v-tooltip>

    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          icon
          class="mr-5"
          v-bind="attrs"
          @click.stop="rightDrawer = !rightDrawer"
          v-on="on"
        >
          <v-icon size="24">mdi-cog-outline</v-icon>
        </v-btn>
      </template>
      <span>Setting</span>
    </v-tooltip>
    <v-btn depressed color="primary" class="mr-8 btn"> Publish </v-btn>
    <v-avatar color="#3b5998" size="36">
      <span class="white--text">VS</span>
    </v-avatar>
  </v-app-bar>
</template>

<script>
export default {
  data() {
    return {
      clipped: true,
      drawer: true,
      fixed: true,
      title: 'autocode',
    }
  },
}
</script>

<style lang="sass">
.app-bar
    box-shadow: 0 0 0 1px #d9d9d999 !important
    & .v-toolbar__title
        font-weight: 800
        letter-spacing: -0.08em !important
        font-size: 21px !important
</style>
