<template>
    <div id="app">
        <v-app id="inspire">
            <v-navigation-drawer v-model="drawer" app clipped>
                <v-list dense>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-home</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title>Dashboard</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-settings</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title>Settings</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </v-navigation-drawer>

            <v-app-bar app clipped-left>
                <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
                <v-toolbar-title>HTTP Monitor</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn class="ma-2" tile color="green" dark>
                    <v-icon left large>mdi-plus</v-icon>
                    Add more
                </v-btn>
            </v-app-bar>

            <v-content>
                <v-container>
                    <v-row>
                        <v-card class="mx-auto" max-width="344" outlined>
                            <Grafico></Grafico>
                        </v-card>
                        <v-card class="mx-auto" max-width="344" outlined>
                            <Grafico></Grafico>
                        </v-card>
                        <v-card class="mx-auto" max-width="344" outlined>
                            <Grafico></Grafico>
                        </v-card>
                    </v-row>
                </v-container>
            </v-content>

            <v-footer app>
                <span>&copy; 2019</span>
            </v-footer>
        </v-app>
        <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
        <!-- <router-view/> -->
    </div>
</template>

<script>
import Vuetify from 'vuetify'
import Grafico from './views/Grafico'


export default {
   vuetify: new Vuetify({
    theme: { dark: true },
  }),
  components:{Grafico},
  data: () => ({
    source: "http://www.google.com.br",
    drawer: false,
  })
}
</script>

<style>

</style>
