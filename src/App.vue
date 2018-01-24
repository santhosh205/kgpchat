<template>
  <v-app>
    <v-toolbar flat dark class="px-3">
      <img src="/static/kgpchat-32x32.png">
      <v-toolbar-title class="pl-2">KGPChat</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-tooltip bottom>
        <v-btn flat icon class="mx-0" slot="activator"
          v-on:click.native.stop="openGroupChatPopup=true">
          <v-icon>group_add</v-icon>
        </v-btn>
        <span>Start a group chat</span>
      </v-tooltip>
      <v-dialog v-model="openGroupChatPopup" 
        scrollable max-width="480px">
        <v-card dark class="py-4">
          <div class="subheading"
            style="width: 100%; 
            text-align: center;">
            Select Members ({{ groupCount }})
          </div>
          <div class="px-5">
            <v-text-field single-line dark
              label="Search Contacts"
              prepend-icon="search"
              color="light-blue lighten-1"
              v-model="groupSearchInput"
              hide-details>
            </v-text-field>
          </div>
          <v-card-text style="height: 360px;">
            <div class="pt-3
              text-xs-center"
              v-show="groupSearchSearching && !groupSearchMatchFound">
              No Results Found
            </div>
            <v-list>
              <v-list-tile
                v-for="(friend, index) in (groupSearchSearching ? groupSearchResults : friends)"
                :key="index+1">
                <v-list-tile-avatar size="32px">
                  <img :src="avatars[index]">
                </v-list-tile-avatar>
                <v-list-tile-title class="body-1">
                  {{ friend }}
                </v-list-tile-title>
                <v-spacer></v-spacer>
                <v-checkbox v-model="groupMembers" 
                  color="light-blue lighten-1" :value="friend" 
                  class="pt-1 pr-1" hide-details>
                </v-checkbox>
              </v-list-tile>
            </v-list>
          </v-card-text>
          <v-card-actions class="px-5">
            <div style="width: 50%;" class="mx-0">
              <v-text-field 
                prepend-icon="group"
                color="light-blue lighten-1"
                placeholder="Group Name"
                class="pt-0" single-line
                hide-details>
              </v-text-field>
            </div>
            <v-spacer></v-spacer>
            <v-btn flat dark color="light-blue lighten-1"
              v-on:click.native="openGroupChatPopup=false">
              Open
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-avatar left size="32px" class="mx-3">
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
        <v-list dark dense>
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
    <v-card dark tile style="height: 100%;">
      <v-layout row wrap style="height: 100%;">
        <v-flex xs3>
          <div class="scroll-div-wrapper">
            <div class="scroll-div px-3">
              <div class="px-3">
                <v-text-field single-line dark
                  label="Search Contacts"
                  prepend-icon="search"
                  color="light-blue lighten-1"
                  v-model="searchInput"
                  hide-details>
                </v-text-field>
              </div>
              <div class="pt-3
                text-xs-center"
                v-show="searching && !matchFound">
                No Results Found
              </div>
              <v-list two-line>
                <v-list-tile ripple
                  v-for="(friend, index) in (searching ? searchResults : friends)"
                  v-on:click="showResult(friend)"
                  :key="index+1">
                  <v-list-tile-avatar size="32px">
                    <img :src="avatars[index]">
                  </v-list-tile-avatar>
                  <v-list-tile-content>
                    <v-list-tile-title 
                      :class="unreadNums[index] > 0 ? ' light-blue--text text--lighten-1' : ''">
                      {{ friend }} {{ unreadNums[index] > 0 ? ('(' + unreadNums[index] + ')') : '' }}
                    </v-list-tile-title>
                    <v-list-tile-sub-title>
                      {{ lastMsgs[index] }}
                    </v-list-tile-sub-title>
                  </v-list-tile-content>
                  <v-list-tile-action>
                    <v-icon v-bind:color="statuses[index] ? 'light-blue lighten-1' : 'grey'">
                      chat_bubble
                    </v-icon>
                  </v-list-tile-action>
                </v-list-tile>
              </v-list>
            </div>
          </div>
        </v-flex>
        <v-flex xs6 class="pa-3" style="height: 100%;">
          <!-- <div class="scroll-div-wrapper">
            <div :class="'scroll-div' + (hideMScrollbar ? ' hide-scroll-bar' : ' ')">
              <div v-for="i in 50" :key="i">Hello {{ i }}</div>
            </div>
          </div> -->
        </v-flex>
        <v-flex xs3>
          <!-- Hello -->
        </v-flex>
      </v-layout>
    </v-card>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        hideMScrollbar: true,
        openGroupChatPopup: false,
        searchInput: '',
        groupSearchInput: '',
        matchFound: false,
        groupSearchMatchFound: false,
        groupName: '',
        groupMembers: [],
        searchResults: [],
        groupSearchResults: [],
        friends: [
          'Santhosh Dasari',
          'Aswanth Kumar',
          'Shreyans Pagariya',
          'Shubham Parekh',
          'Gaurav Jain',
          'Kshitij Kumar'
        ],
        lastSeens: [
          '1 min ago',
          '20 min ago',
          '3 days ago',
          '1 week ago',
          '3 weeks ago',
          'Dec 2017'
        ],
        lastMsgs: [
          'Hi, bro',
          'How are you?',
          'I\'m fine',
          'Yo mama',
          'Ela unnav',
          'Nenu super ra'
        ],
        unreadNums: [2, 2, 1, 0, 0, 0],
        statuses: [true, false, true, true, false, false],
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
        this.searchResults = []
        if (this.searchInput !== '') {
          this.friends.forEach((friend) => {
            if (friend.toLowerCase().includes(this.searchInput.toLowerCase())) {
              this.searchResults.push(friend)
            }
          })
        }
        if (this.searchResults.length !== 0) {
          this.matchFound = true
        } else {
          this.matchFound = false
        }
      },
      groupSearchInput (value) {
        this.groupSearchResults = []
        if (this.groupSearchInput !== '') {
          this.friends.forEach((friend) => {
            if (friend.toLowerCase().includes(this.groupSearchInput.toLowerCase())) {
              this.groupSearchResults.push(friend)
            }
          })
        }
        if (this.groupSearchResults.length !== 0) {
          this.groupSearchMatchFound = true
        } else {
          this.groupSearchMatchFound = false
        }
      }
    },
    computed: {
      // v-text-field -> :loading="loadingSearch"
      // loadingSearch () {
      //   if (this.searchInput !== '') {
      //     return true
      //   } else {
      //     return false
      //   }
      // }
      groupCount () {
        return this.groupMembers.length + 1
      },
      searching () {
        if (this.searchInput !== '') {
          return true
        } else {
          return false
        }
      },
      groupSearchSearching () {
        if (this.groupSearchInput !== '') {
          return true
        } else {
          return false
        }
      }
    },
    methods: {
      showResult (name) {
        console.log(name)
      },
      selectOption () {
        console.log('Option Selected!')
      }
    }
  }
</script>

<style scoped>
  .scroll-div-wrapper {
    position: relative;
    height: 100%;
  }
  .scroll-div {
    position: absolute;
    top: 0px; bottom: 0px;
    left: 0px; right: 0px;
    overflow: auto;
  }
  .hide-scroll-bar::-webkit-scrollbar {
    display: none;
  }
</style>
