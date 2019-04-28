<template>
  <v-app dark>
    <v-content class="mt-4">
      <v-container>
        
        <!-- Jumbotron starts -->
        <v-layout v-if="!started">
          <v-flex xs11 lg12>
            <v-jumbotron>
              <v-container fill-height>
                <v-layout align-center>
                  <v-flex>
                    <h3 class="display-3">        
                      <span class="logo-post-text">REPO</span>
                      <span class="logo-pre-text">METRICS</span>
                    </h3>
                    <span class="subheading">is a tool to analyse and compare activity levels on open source projects hosted on GitHub.</span>
                    <v-divider class="my-3"></v-divider>
                    <v-text-field
                      class="mt-3 auth-field"
                      v-model="username"
                      label="GitHub Username"
                      persistent-hint
                    ></v-text-field>
                    <v-text-field
                      class="mt-3 auth-field"
                      v-model="password"
                      label="GitHub Password"
                      hint="Not sent / saved anywhere. Only used for GitHub API authentication."
                      persistent-hint
                      mask
                      type="password"
                      @keyup.enter="started=true"
                    ></v-text-field>
                    <v-btn
                      class="mt-4 ml-0"
                      color="primary"
                      large
                      @click="started=true"
                      :disabled="!(username.length && password.length)"
                    >
                      Start Analysing
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-jumbotron>
          </v-flex>
        </v-layout>
        <!-- Jumbotron ends -->

        <template v-if="started">
          <!-- Header starts -->
          <v-layout>
            <v-flex>
              <h3 class="display-3">        
                <span class="logo-post-text">REPO</span>
                <span class="logo-pre-text">METRICS</span>
              </h3>
              <span class="subheading">is a tool to analyse and compare activity levels on open source projects hosted on GitHub.</span>
            </v-flex>
          </v-layout>
          <!-- Header ends -->
          
          <!-- Searchbar starts -->
          <v-layout mt-4>
            <v-flex>
              <search-bar :username="username" :password="password" v-model="projects"></search-bar>
            </v-flex>
          </v-layout>
          <!-- Searchbar ends -->
          
          <!-- Graphs starts -->
          <v-layout mt-2>
            <v-flex xs12 lg8>
              <commit-graph :projects="projects" :key="key"></commit-graph>
            </v-flex>
            <v-flex xs12 lg4>
              <count-graph :projects="projects" :key="key"></count-graph>
            </v-flex>
          </v-layout>
          <!-- Graphs end -->
          
          <!-- Footer starts -->
          <v-layout mt-4>
            <v-flex xs12>
              <h3>
                <span class="logo-post-text">REPO</span>
                <span class="logo-pre-text">METRICS</span> is a visual representation of activities in a GitHub repository. It helps you decide / settle on an open source project by showing if it's being actively worked upon or not.
              </h3>
              <p>
                Made with ❤ using
                <a class="footer-link" href="https://vuetifyjs.com">Vuetify</a> and
                <a class="footer-link" href="https://developer.github.com/v3/">GitHub API V3</a>, and some amounts of an Easter weekend. <a class="footer-link" href="https://gitlab.com/joelewis/codemetrics">Open-sourced @ GitLab</a>.
              </p>
            </v-flex>
          </v-layout>
          <!-- Footer ends -->
        </template>
      
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
            started: false,
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
.footer-link {
  color:coral !important;
}
.logo-pre-text {
    font-weight: 400;
}
.logo-post-text {
    font-weight: 900;
}
.main-toolbar {
    height: 80px;
}
.auth-field {
  width: 300px;
}

</style>