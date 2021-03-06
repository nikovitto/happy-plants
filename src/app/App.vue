<template>
  <v-touch
    id="app"
    @swiperight="onSwipeRight"
    @swipeleft="onSwipeLeft"
    :swipe-options="{ direction: 'horizontal' }">
    <app-notifications
      class="notifications"
      :message="message">
    </app-notifications>
    <router-view></router-view>
  </v-touch>
</template>

<script>
  import { mapActions, mapState } from 'vuex'
  import AppNotifications from '@/components/AppNotifications'

  export default {
    name: 'HappyPlants',

    components: {
      'app-notifications': AppNotifications
    },

    data () {
      return {
        notificationTimeout: 2000
      }
    },

    computed: mapState({
      authenticated: state => state.user.authenticated,
      message: state => state.notification.message
    }),

    methods: {
      ...mapActions([
        'loadPlants',
        'loadSettings',
        'loadCategories',
        'hideNotification',
        'signInUser',
        'signOutUser',
        'authError'
      ]),
      isOverviewRoute () {
        return (
          this.$route.name === 'Overview' ||
          this.$route.path === '/'
        )
      },
      onSwipeRight () {
        if (this.isOverviewRoute()) {
          return
        }
        this.$router.back()
      },
      onSwipeLeft () {
        if (!this.isOverviewRoute()) {
          return
        }
        this.$router.push('/add')
      }
    },

    mounted () {
      Promise.all([
        this.loadSettings(),
        this.loadPlants(),
        this.loadCategories()
      ])
    },

    updated () {
      if (this.message) {
        setTimeout(this.hideNotification, this.notificationTimeout)
      }
    }
  }
</script>

<style lang="scss">
  @import "~node_modules/normalize.css/normalize";
  @import "~styles/colors";
  @import "~styles/fonts";
  @import "~styles/typography";
  @import "~styles/forms";
  @import "~styles/buttons";
  @import "~styles/layout";
  @import "~styles/z-index";

  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  body {
    display: flex;
    justify-content: center;

    &.js-no-scrolling {
      overflow: hidden;
    }

    /* TODO: Remove when desktop layout is actually in development. */
    @media (min-width: var(--app-media-max-size)) {
      background: var(--brand-green);
    }
  }

  #app {
    width: 100vw;
    max-width: 600px;
    min-height: 100vh;
    height: 100%;

    /* TODO: Remove when desktop layout is actually in development. */
    @media (min-width: var(--app-media-max-size)) {
      background: var(--background-primary);
      height: 450px;
      box-shadow: 0 0 20px var(--transparency-black-light);
    }
  }

  .notifications {
    z-index: z($page-elements, notifications);
  }

  .main-wireframe {
    display: flex;
    height: 100%;
    min-height: 100vh;
    flex-direction: column;
    justify-content: flex-start;
    align-items: stretch;
    padding-top: var(--app-header-size);
  }

  .svg-icon {
    display: inline-block;
    width: 16px;
    height: 16px;
    color: inherit;
    vertical-align: middle;
    fill: none;
    stroke: currentColor;

    path {
      fill: currentColor;
    }
  }

  .svg-fill {
    fill: currentColor;
    stroke: none;
  }
</style>
