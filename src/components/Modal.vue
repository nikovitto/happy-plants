<template>
  <section v-if="show" :style="{ backgroundColor }">
    <header class="modal-header">
      <button
        aria-label="Close"
        class="circle inverse"
        @click.prevent="emitModalClose">
        <feather-x width="18" height="18" />
      </button>
      <slot name="headline"></slot>
    </header>
    <slot name="content"></slot>
  </section>
</template>

<script>
  export default {
    name: 'Modal',

    props: {
      show: { type: Boolean, default: false },
      backgroundColor: { type: String, default: '#F5F5F5' }
    },

    components: {
      'feather-x': () =>
        import('vue-feather-icon/components/x' /* webpackChunkName: "general" */)
    },

    watch: {
      show () {
        if (this.show) {
          this.$root.$el.parentNode.classList.add('js-no-scrolling')
        } else {
          this.$root.$el.parentNode.classList.remove('js-no-scrolling')
        }
      }
    },

    methods: {
      emitModalClose () {
        this.$emit('close-modal')
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import "~styles/z-index";

  section {
    width: 100%;
    min-height: 100vh;
    height: 100%;
    background: var(--background-secondary);
    position: fixed;
    top: 0;
    left: 0;
    padding: var(--base-gap);
    z-index: z($page-elements, modals);
    overflow-y: scroll;
  }

  .modal-header {
    display: flex;
    align-items: flex-start;
    margin-bottom: var(--base-gap);

    button {
      margin-right: calc(var(--base-gap) + 5px);
      background: var(--background-primary);
      color: var(--link-color);
      box-shadow: var(--plain-shadow);
    }

    h1 {
      font-size: var(--text-size-large);
      font-weight: 600;
      line-height: 115%;
      margin-top: 8px;
    }
  }
</style>
