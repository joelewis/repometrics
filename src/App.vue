<template>
  <v-app dark>
    <v-toolbar dark class="main-toolbar" ma-4 app>
        <v-toolbar-title>
          <span class="logo-post-text">REPO</span><span class="logo-pre-text">METRICS</span>
        </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-text-field
        class="mt-3"
        v-model="username"
        label="Username"
        hint="Your github username. Will be used for Basic Authentication."
        persistent-hint
      ></v-text-field>
      <v-spacer></v-spacer>
      <v-text-field
        class="mt-3"
        v-model="password"
        label="Password"
        hint="Your github password. Will be used for Basic Authentication."
        persistent-hint
        mask
        type="password"
      ></v-text-field>
    </v-toolbar>

    <v-content class="mt-4">
      <v-container>
        <v-layout>
          <v-flex>
            <search-bar :username="username" :password="password" v-model="projects"></search-bar>
          </v-flex>
        </v-layout>
        <v-layout>
          <v-flex xs12 lg8>
            <score-graph :projects="projects" :key="key"></score-graph>
          </v-flex>
          <v-flex xs12 lg4>
            <count-graph :projects="projects" :key="key"></count-graph>
          </v-flex>
        </v-layout>
        <v-layout mt-4>
          <v-flex xs12>
            <h3><span class="logo-post-text">REPO</span><span class="logo-pre-text">METRICS</span> helps you to check the activity on Github repos, before deciding to adopt the open source project in your product.</h3>
            <p>It uses your GitHub username/password to make authenticated requests from the browser directly to GitHub. If you don't provide your credentials you will eventually run into rate-limiting problems. Made with ❤ using <a href="vuetify.com">Vueitfy</a> and <a href="https://developer.github.com/v3/"> GitHub API V3 </a>, and some amounts of an Easter weekend.</p>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import SearchBar from './components/SearchBar'
import CountGraph from './components/CountGraph'
import ScoreGraph from './components/ScoreGraph'

export default {
  name: 'App',
  components: {
    SearchBar,
    CountGraph,
    ScoreGraph
  },
  data () {
    return {
      username: '',
      password: '',
      projects: [],
      key: ""
    }
  },

  watch: {
    projects (projects) {
      // get 
      this.key = projects.map(project => project.id).join('-');
    }
  }
}
</script>

<style>
.logo-pre-text {
  font-weight: 400;
}
.logo-post-text {
  font-weight: 900;
}
.main-toolbar {
  height: 80px;
}
</style>