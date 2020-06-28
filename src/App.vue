<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      clipped
      fixed
      app
    >
      <v-list dense>
        <v-list-tile v-for="(item, i) in menu" :key="i" :to="item.link">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app fixed clipped-left dense>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>D3VegaVue</v-toolbar-title>
      <v-spacer></v-spacer>
      <!-- <v-btn light target="new" href="https://github.com/tedy69/D3-Vue">
        <img class="mr-2" src="/static/img/github.svg" height="90%" alt="github">
        <span>github</span>
      </v-btn> -->
    </v-toolbar>
    <v-content>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>
    <v-footer app fixed>
      <v-flex xs12 text-xs-center>
        <span>Belajar vue.js dengan d3.js by Mochammad Tedy Fazrin <a target="new" href="https://riseofdevelopers.tech">riseofdevelopers.tech</a> &copy; 2020</span>
      </v-flex>
    </v-footer>
    <v-snackbar
      :timeout="notification.timeout"
      top
      :value="notification.activo"
      :color="notification.color"
    >
      {{ notification.text }}
      <v-btn flat color="yellow"
      @click.native="handleNotification">
        {{ notification.btnText }}
      </v-btn>
    </v-snackbar>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: true,
      menu: [
        {link: '/', title: 'Dashboard', icon: 'dashboard'},
        {link: '/pie-chart', title: 'Pie Chart', icon: 'donut_large'},
        {link: '/map-chart', title: 'Map', icon: 'map'},
        {link: '/network-graph', title: 'Network Graph', icon: 'bubble_chart'},
        {link: '/area-chart', title: 'Area Chart', icon: 'timeline'},
        {link: '/bar-chart', title: 'Bar Chart', icon: 'bar_chart'},
        {link: '/cluster-dendogram', title: 'Cluster Dendogram', icon: 'line_style'},
        {link: '/testing', title: 'Testing', icon: 'dashboard'},
        {link: '/vega', title: 'Vega', icon: 'dashboard'}
      ]
    }),
    methods: {
      handleNotification () {
        if (this.notification.btnText === 'reload') {
          location.reload(true)
        } else {
          this.$store.commit('notification', {
            activo: false,
            text: '',
            timeout: null,
            color: null,
            btnText: 'close'
          })
        }
      }
    },
    computed: {
      notification () {
        return this.$store.getters.notification
      }
    }
  }
</script>