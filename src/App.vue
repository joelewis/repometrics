<template>
  <v-app dark>
    <v-toolbar dark class="main-toolbar" ma-4 app>
      <v-toolbar-title>
        <span class="logo-post-text">REPO</span>
        <span class="logo-pre-text">METRICS</span>
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
            <commit-graph :projects="projects" :key="key"></commit-graph>
          </v-flex>
          <v-flex xs12 lg4>
            <count-graph :projects="projects" :key="key"></count-graph>
          </v-flex>
        </v-layout>
        <v-layout mt-4>
          <v-flex xs12>
            <h3>
              <span class="logo-post-text">REPO</span>
              <span class="logo-pre-text">METRICS</span> is a visual representation of activities in a GitHub repository. It helps you decide / settle on an open source project by showing if it's being actively worked upon or not.
            </h3>
            <p>
              It uses your GitHub username/password to make authenticated requests from the browser directly to GitHub. If you don't provide your credentials you will eventually run into rate-limiting problems. Made with ❤ using
              <a href="https://vuetifyjs.com">Vuetify</a> and
              <a href="https://developer.github.com/v3/">GitHub API V3</a>, and some amounts of an Easter weekend.
            </p>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import SearchBar from './components/SearchBar'
import CountGraph from './components/CountGraph'
import CommitGraph from './components/CommitGraph'

export default {
    name: 'App',
    components: {
        SearchBar,
        CountGraph,
        CommitGraph,
    },
    data() {
        return {
            username: '', // for basic authorization header
            password: '', // for basic authorization header
            projects: [], // list of github projects selected
            key: '', // to re-render graphs when `this.projects` get updated
        }
    },

    watch: {
        // watch projects and reset this.key to re-render graphs
        projects(projects) {
            this.key = projects.map(project => project.id).join('-')
        },
    },
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