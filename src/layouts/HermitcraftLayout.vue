<template>
  <q-layout view="lHh LpR lFr">
    <q-toolbar class="font-open-sans">
      <q-btn 
        flat 
        round 
        dense 
        icon="r_menu" 
        @click="leftDrawerOpen = !leftDrawerOpen"
      />
      <q-space />
      <q-btn flat stretch label="Back to post" />
    </q-toolbar>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      content-class="bg-grey-1 text-grey"
    >
      <q-list class="text-black">
        <q-item-label header class="font-open-sans">
          2020 - r_coder - demo<br><hr>
          <span class="text-red">
            Disclaimer: I am not connected with Hermitcraft and its members. 
            This is a concept web design for Hermitcraft.com, focused on mobile optimization.
          </span>
        </q-item-label>

        <q-item 
          clickable 
          v-ripple 
          class="text-black"
          :to="getHomePage()"
        >
          <q-item-section avatar>
            <q-icon name="r_home" />
          </q-item-section>

          <q-item-section class="text-weight-bold text-uppercase">
            Home
          </q-item-section>
        </q-item>

        <q-item 
          clickable 
          v-ripple 
          v-if="isHermitListLoaded"
          class="text-red-10" 
          :to="getHomePage() + 'livestream'"
        >
          <q-item-section avatar>
            <q-icon color="red-10" name="r_live_tv" />
          </q-item-section>

          <q-item-section class="text-weight-bold text-uppercase">
            Hermitcraft Live
          </q-item-section>
        </q-item>

        <q-item 
          clickable 
          v-ripple 
          v-if="isHermitListLoaded"
          class="text-primary" 
          :to="getHomePage() + 'timeline'"
        >
          <q-item-section avatar>
            <q-icon color="primary" name="r_history" />
          </q-item-section>

          <q-item-section class="text-weight-bold text-uppercase">
            Hermitcraft Timeline
          </q-item-section>
        </q-item>
        
        <q-expansion-item
          expand-separator
          v-if="isHermitListLoaded"
        >
          <template v-slot:header>
            <q-item-section avatar>
              <q-icon name="r_how_to_reg" />
            </q-item-section>

            <q-item-section class="text-weight-bold text-uppercase">
              Active
            </q-item-section>

            <q-item-section side>
              <div class="row items-center" v-if="displayActiveTwitchStreams()">
                <q-avatar class="live-on-twitch" size="sm" text-color="white">
                  {{ activeTwitchStreams }}
                </q-avatar>
              </div>
            </q-item-section>
          </template>

          <q-card>
            <q-card-section class="q-pa-none font-open-sans">
              <q-list>
                <q-item 
                  v-for="hermit in hermitsOrganized.active" 
                  :key="hermit.channel.youTube"
                  :to="getHomePage() + 'hermit/' + hermit.username"
                  clickable 
                  v-ripple
                >
                  <q-item-section avatar>
                    <q-avatar>
                      <img :src="hermit.accountPicture" />
                    </q-avatar>
                  </q-item-section>
                  <q-item-section>
                    {{hermit.name}}
                  </q-item-section>
                  <q-item-section side v-if="hermit.livestreams.twitch">
                    <q-item-section v-if="hermit.livestreams.twitch.isChannelLive">
                      <q-badge class="live-on-twitch" label="LIVE" />
                    </q-item-section>
                  </q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
          </q-card>
        </q-expansion-item>

        <q-expansion-item
          expand-separator
          v-if="isHermitListLoaded"
        >
          <template v-slot:header>
            <q-item-section avatar>
              <q-icon name="r_person" />
            </q-item-section>

            <q-item-section class="text-weight-bold text-uppercase">
              Inactive
            </q-item-section>
          </template>

          <q-card>
            <q-card-section class="q-pa-none font-open-sans ">
              <q-list>
                <q-item 
                  v-for="hermit in hermitsOrganized.inactive" 
                  :key="hermit.channel.youTube"
                  clickable 
                  v-ripple
                >
                  <q-item-section avatar>
                    <q-avatar>
                      <img :src="hermit.accountPicture" />
                    </q-avatar>
                  </q-item-section>

                  <q-item-section>
                    {{hermit.name}}
                  </q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
          </q-card>
        </q-expansion-item>

        <q-expansion-item 
          expand-separator
          v-if="isHermitListLoaded"
        >
          <template v-slot:header>
            <q-item-section avatar>
              <q-icon name="r_link" />
            </q-item-section>

            <q-item-section class="text-weight-bold text-uppercase">
              Links
            </q-item-section>
          </template>

          <q-card>
            <q-card-section class="q-pa-none font-open-sans">
              <q-list class="font-open-sans">
                <q-item clickable v-ripple>
                  <q-item-section>Hermitcraft Seeds</q-item-section>
                </q-item>
                <q-item 
                  clickable 
                  v-ripple
                  :to="getHomePage() + 'recap'"
                >
                  <q-item-section>
                    Hermitcraft Recap
                    <q-item-label class="text-caption">With Pixlriffs &amp; ZloyXP</q-item-label>
                  </q-item-section>
                </q-item>
                <q-item clickable v-ripple>
                  <q-item-section>Hermitcraft on Reddit</q-item-section>
                </q-item>
                <q-item clickable v-ripple>
                  <q-item-section>Hermitcraft on Twitter</q-item-section>
                </q-item>
                <q-item clickable v-ripple>
                  <q-item-section>Hermitcraft on Twitch</q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
          </q-card>
        </q-expansion-item>

        <q-expansion-item 
          expand-separator
          v-if="isHermitListLoaded"
        >
          <template v-slot:header>
            <q-item-section avatar>
              <q-icon name="r_maps" />
            </q-item-section>

            <q-item-section class="text-weight-bold text-uppercase">
              Maps
            </q-item-section>
          </template>

          <q-card>
            <q-card-section class="q-pa-none font-open-sans">
              <q-list class="font-open-sans">
                <q-item 
                  clickable 
                  v-ripple
                  v-for="items in seasonMaps"
                  :key="items.index"
                >
                  <q-item-section avatar>
                    <q-avatar :color="items.icon.color" text-color="white" size="30px">
                      {{ items.icon.caption }}
                    </q-avatar>
                  </q-item-section>
                  <q-item-section>
                    {{ items.name }}
                  </q-item-section>
                </q-item>
              </q-list>
            </q-card-section>
          </q-card>
        </q-expansion-item>

        <q-item v-else>
          <q-item-section>
            <q-skeleton animation="wave" />
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <div class="main-container">
        <q-page-container class="no-padding">
          <router-view />
        </q-page-container>
      </div>
    </q-page-container>
  </q-layout>
</template>

<style scoped>
  .main-container {
    min-width: 300px;
  }
</style>

<script>
  import s6Maps from '../data/data-season-maps.js'

  import {
    fetchHermitChannels,
    getHermitChannels
  } from '../data/api/hermits.js'

  import {
    fetchLiveActiveHermits,
    getLiveActiveHermits,
  } from '../data/api/livestream.js'

  export default {
    name: 'HermitcraftLayout',

    data () {
      return {
        activeTwitchStreams: 0,
        activeYouTubeStreams: 0,
        leftDrawerOpen: false,
        loadingHermitsComplete: false,
        hermitsOrganized: {},
        isHermitListLoaded: false,
        seasonMaps: [],
        fetchLiveActiveHermitsInterval: null,
      }
    },

    created () {
      this.seasonMaps = s6Maps;

      this.executeFetchChannels();
    },

    methods: {
      externalRedirect(href) {
        window.open(href, '_blank');
      },

      showNotification(name, profile, platform) {
        this.$q.notify({
          message: `${name} is live now on ${platform}!`,
          position: 'bottom-right',
          classes: `font-open-sans`,
          avatar: profile,
          timeout: 10000,
          actions: [
            { label: 'Watch Now!', color: 'white', handler: () => { /* ... */ } },
          ]
        })
      },

      countActiveStreams(item) {
        if (item.livestreams.twitch.isChannelLive) {
          this.activeTwitchStreams++;

          this.showNotification(
            item.name, 
            item.accountPicture,
            'Twitch',
          );
        }
      },

      displayActiveTwitchStreams() {
        return this.activeTwitchStreams || false
      },

      organizeHermits() {
        const data = getHermitChannels();

        let tempHermits = {
          active: [],
          inactive: []
        };

        data.map(item => {
          item["livestreams"] = {};

          if (item.status === "active") {
            tempHermits.active.push(item);
          } else {
            tempHermits.inactive.push(item);
          }
        });

        this.hermitsOrganized = tempHermits;
        this.isHermitListLoaded = true;

        if (this.isHermitListLoaded)
          this.executeFetchLiveActiveHermitsInterval();
      },

      executeFetchChannels() {
        fetchHermitChannels().then(() => this.organizeHermits())
      },

      getLivestreamData(livestreamData, channelData) {
        const index = livestreamData.findIndex(
          status => status.channel.youtube === channelData.youtube && 
                      status.channel.twitch === channelData.twitch
        );

        return livestreamData[index].livestreams;
      },

      organizeHermitLivestreamData() {
        const _lsData = getLiveActiveHermits();
        
        this.activeTwitchStreams = 0;
        this.hermitsOrganized.active.map(item => {
          item.livestreams = this.getLivestreamData(_lsData, item.channel);

          this.countActiveStreams(item);
        });
      },

      executeFetchLiveActiveHermits() {
        fetchLiveActiveHermits().then(() => this.organizeHermitLivestreamData());
      },

      executeFetchLiveActiveHermitsInterval() {
        this.executeFetchLiveActiveHermits();

        this.fetchLiveActiveHermitsInterval = setInterval(() => {
          this.executeFetchLiveActiveHermits()
        }, 150000);
      },

      getHomePage() {
        return '/';
      }
    }
  }
</script>