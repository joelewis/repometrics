<template>
  <v-autocomplete
    v-model="selectedProjects"
    :loading="isLoading"
    :items="searchResult"
    box
    chips
    color="blue-grey lighten-2"
    label="Search project in GitHub"
    item-text="full_name"
    item-value="id"
    :search-input.sync="search"
    multiple
    hide-no-data
  >
    <template v-slot:selection="data">
      <v-chip :selected="data.selected" close class="chip--select-multi" @input="remove(data.item)">
        <v-avatar color="grey darken-3">
          <span class="white--text headline">{{ data.item.name.charAt(0).toUpperCase() }}</span>
        </v-avatar>
        {{ data.item.full_name }}
      </v-chip>
    </template>
    <template v-slot:item="data">
      <template>
        <v-list-tile-avatar color="grey darken-3">
          <span class="white--text headline">{{ data.item.name.charAt(0).toUpperCase() }}</span>
        </v-list-tile-avatar>
        <v-list-tile-content>
          <v-list-tile-title v-html="data.item.full_name"></v-list-tile-title>
          <v-list-tile-sub-title v-html="data.item.description"></v-list-tile-sub-title>
        </v-list-tile-content>
      </template>
    </template>
  </v-autocomplete>
</template>
<script>
// default exports
export default {
    props: {
        projects: {
            type: Array,
            default: function() {
                return []
            },
        },

        username: {
            type: String,
            default: '',
        },

        password: {
            type: String,
            default: '',
        },
    },

    data: function() {
        return {
            selectedProjects: [], 
            searchResult: [], 
            searchCache: {}, // a cache of already fetched search results. Used to uniquify redundant suggestions.
            search: null,
            isLoading: false,
        }
    },

    watch: {
        // watch search input & update this.searchResult by searching through github API
        search: function(val) {
            this.isLoading = true
            // debounce requests to github to minimize number of requests.
            this.debounce(this.fetchSuggestions.bind(this, val), 1500);
        },

        selectedProjects: function(selectedProjects) {
            var self = this
            self.isLoading = true
            
            // TODO: O(scary). Optimize by maintaining a hashmap maybe?
            var projects = selectedProjects.map(function(repoId) {
                return this.getRepoForId(repoId)
            }, this)

            // fetch commit logs for a year for each of the project.
            Promise.all(
                projects.map(project =>
                    fetch(
                        `https://api.github.com/repos/${project.full_name}/stats/commit_activity`,
                        {
                            headers: {
                                Authorization: `Basic ${btoa(
                                    this.username + ':' + this.password
                                )}`,
                            },
                        }
                    )
                        .then(resp => resp.json())
                        .then(resp => {
                            // TODO: this is to workaround a bug in github API when it returns an empty object
                            // instead of an array of stats. Need to find a better solution.
                            var resp = resp.length ? resp : []
                            project.commits = resp
                        })
                        .catch(resp => console.log(resp)) // TODO: properly handle errors
                )
            ).then(data => {
                self.isLoading = false
                this.$emit('input', projects)
            })
        },
    },

    methods: {
        remove: function(repo) {
            this.selectedProjects = this.selectedProjects.filter(
                repoId => repoId != repo.id
            )
        },

        getRepoForId: function(id) {
            for (var i = 0; i < this.searchResult.length; i++) {
                var project = this.searchResult[i]
                if (project.id === id) {
                    return project
                }
            }
            throw new Error(
                `github repo with id ${id} cannot be found in cached items`
            )
        },

        fetchSuggestions: function(val) {
            var self = this;
            if (!val) {
                self.isLoading = false
                return;
            }
            // make request
            var query = encodeURI(val)
            fetch(
                `https://api.github.com/search/repositories?q=${query}&sort=stars`,
                {
                    // TOTHINK: convert to JS template strings?
                    headers: {
                        Authorization: `Basic ${btoa(
                            self.username + ':' + self.password
                        )}`,
                    },
                }
            )
                .then(resp => {
                    return resp.json()
                })
                .then(repos => {
                    self.isLoading = false
                    // add repos to searchCache, and then finally extract searchCache.values
                    repos.items.forEach(repo => {
                        self.searchCache[repo.id] = repo
                    })
                    self.searchResult = Object.values(self.searchCache)
                }) // TODO: properly handle errors
        },

        debounce: (function() { // A closure over timeoutId to track previous timeout.
            var timeoutId = null;
            return function(fn, time) {
                clearTimeout(timeoutId);
                timeoutId = setTimeout(fn, time);
            }
        })()
    },
}
</script>