<template>
  <v-app>
    <v-toolbar dark>
        <v-toolbar-title>
          <span class="logo-post-text">CODE</span><span class="logo-pre-text">BEAT</span>
        </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
      <v-container>
        <v-layout>
          <v-flex>
            <search-bar v-model="projects"></search-bar>
          </v-flex>
        </v-layout>
        <v-layout>
          <v-flex>
            <commit-graph :projects="projects"></commit-graph>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import SearchBar from './components/SearchBar'
import CommitGraph from './components/CommitGraph'

export default {
  name: 'App',
  components: {
    SearchBar,
    CommitGraph
  },
  data () {
    return {
      projects: [],
      examples: [
        {label: 'Frontend Frameworks', projects: ['facebook/react', 'google/angular']},
        {label: 'Go Web Frameworks', projects: ['gorilla/gorilla']}
      ]
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

</style>