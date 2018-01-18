<template>
  <v-app>
    <v-toolbar flat color="white" 
      light dense class="px-5">
      <img src="/static/kgpchat-32x32.png">
      <v-toolbar-title>KGPChat</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-menu flat bottom offset-y full-width
        content-class="elevation-1"
        transition="slide-y-transition"
        :close-on-content-click="false"
        v-model="open" class="mr-2">
        <v-text-field color="light-blue"
          slot="activator" label="Search"
          append-icon="search"
          v-model="searchInput"
          hide-details single-line
          v-on:click.stop="open = true"
          v-on:click.native="open = false">
        </v-text-field>
        <v-list v-show="matchFound"
          dense>
          <v-list-tile
            v-for="(result, index) in results"
            v-on:click="showResult(index)"
            :key="index+1">
            <v-list-tile-avatar size="24px">
              <img :src="avatars[index]">
            </v-list-tile-avatar>
            <v-list-tile-title class="body-1"
              style="margin-left: -16px;">
              {{ result }}
            </v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
      <v-tooltip bottom>
        <v-btn flat icon class="mx-2"
          slot="activator">
          <v-icon>group_add</v-icon>
        </v-btn>
        <span>Start a group chat</span>
      </v-tooltip>
      <v-avatar left size="24px" class="mx-2">
        <img src="/static/nina-32x32.jpg">
      </v-avatar>
      <div class="body-2">Nina Dobrev</div>
      <v-menu bottom offset-y left
        transition="slide-y-transition"
        offset-x :nudge-right="28"
        content-class="elevation-1">
        <v-btn flat small icon slot="activator">
          <v-icon>arrow_drop_down</v-icon>
        </v-btn>
        <v-list dense>
          <v-list-tile
            v-on:click="selectOption">
            <v-list-tile-title
              class="body-1">
              Account
            </v-list-tile-title>
          </v-list-tile>
          <v-list-tile
            v-on:click="selectOption">
            <v-list-tile-title
              class="body-1">
              Settings
            </v-list-tile-title>
          </v-list-tile>
          <v-list-tile
            v-on:click="selectOption">
            <v-list-tile-title
              class="body-1">
              Logout
            </v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-menu>
    </v-toolbar>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        searchInput: '',
        open: false,
        matchFound: false,
        results: [],
        matches: [
          'Santhosh Dasari',
          'Aswanth Kumar',
          'Shreyans Pagariya',
          'Shubham Parekh',
          'Gaurav Jain',
          'Kshitij Kumar'
        ],
        avatars: [
          '/static/ariel-32x32.jpg',
          '/static/brave-32x32.jpg',
          '/static/jasmine-32x32.jpg',
          '/static/mulan-32x32.jpg',
          '/static/pocahontas-32x32.jpg',
          '/static/snow-white-32x32.jpg'
        ]
      }
    },
    watch: {
      searchInput (value) {
        this.results = []
        if (this.searchInput !== '') {
          this.matches.forEach((match) => {
            if (match.toLowerCase().includes(this.searchInput.toLowerCase())) {
              this.results.push(match)
            }
          })
        }
        if (this.results.length !== 0) {
          this.matchFound = true
        } else {
          this.matchFound = false
        }
      }
    },
    // v-text-field -> :loading="loadingSearch"
    // computed: {
    //   loadingSearch () {
    //     if (this.searchInput !== '') {
    //       return true
    //     } else {
    //       return false
    //     }
    //   }
    // },
    methods: {
      showResult (index) {
        console.log(this.results[index])
      },
      selectOption () {
        console.log('Option Selected!')
      }
    }
  }
</script>

<style scoped>

</style>
