<template>
  <v-app>
    <v-navigation-drawer
      temporary
      :mini-variant="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      enable-resize-watcher
      fixed
      dark
      class="primary lighten-3"
      app
      v-if="!isLanding"
      v-on:close-sidebar="isActive = !isActive"
    >
      <v-list dark class="full-height cover">
        <v-list-tile avatar>
          <v-list-tile-avatar>
            <img :src="`./static/img/icons/physics-concepts/005-physics.png`" alt="John">
          </v-list-tile-avatar>

          <v-list-tile-content>
            <v-list-tile-title>Santiago</v-list-tile-title>
            <v-list-tile-sub-title>Level 12 (2280 points)</v-list-tile-sub-title>
          </v-list-tile-content>


          <v-list-tile-action>
            <v-btn icon>
              <v-icon>edit</v-icon>
            </v-btn>
          </v-list-tile-action>
        </v-list-tile>

        <v-divider></v-divider>

        <v-list-tile
          v-for="item in menu"
          :key="item.title"
          @click="visit(item)"
          class="sidebar-menu--item"
          :class="{'active' : item.active}"
        >

            <v-list-tile-action>
              <v-icon v-html="item.icon" color="white"></v-icon>
            </v-list-tile-action>

            <v-list-tile-content>
              <v-list-tile-title v-html="item.title"></v-list-tile-title>
            </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app color="accent" dark :class="{'elevation-0': $route.name === 'Home'}" v-if="!isLanding">
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>

      <v-toolbar-title class="ml-0">{{activeEl.title === 'Home' ? 'Physics' : activeEl.title}}</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-toolbar-items>
        <v-btn fab flat>
          <v-icon>{{activeEl.icon}}</v-icon>
        </v-btn>
      </v-toolbar-items>

    </v-toolbar>
    <v-content>
      <router-view/>
    </v-content>

    <!-- <v-footer :fixed="fixed" app>
      <span> \(\dfrac {\alpha} { \beta}\)</span>
    </v-footer> -->
  </v-app>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Physiks'
    }
  },

  created () {
    this.fetchModules()
  },

  computed: {
    isLanding () {
      return this.$route.name === 'Landing'
    },
    ...mapGetters('Navigation', ['menu']),
    ...mapGetters('User', ['user']),
    activeEl: function () {
      return this.menu.filter(item => item.active)[0]
    }
  },

  methods: {
    ...mapActions('ModulesIndex', ['fetchModules']),
    ...mapActions('Navigation', ['goto']),
    ...mapActions('User', ['fetchUser']),
    visit (item) {
      this.goto({routeName: item.routeName, activeEl: item})
    }
  },

  mounted () {
    this.fetchUser()
  },
  name: 'App'
}
</script>

<style>
.rounded-card {
    border-radius: 1rem;
    overflow: hidden;
  }

  .mw-50 {
    max-width: 50%;
  }

  .mh100 {
    min-height: 100vh;
  }

  aside.v-navigation-drawer {
    background-image: url('/static/img/364.jpg');
    background-size: cover;
    background-position: center;
  }

  .full-height.cover {
    background-color: rgba(49,27,146,.8);
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }


.sidebar-menu--item.active {
  background: rgba(255,255,255,0.08);
}

.relative {
  position: relative;
}

.rounded-0{
  border-radius: 0 !important;
}

.rounded-all {
  border-radius: 50%;
}

</style>
