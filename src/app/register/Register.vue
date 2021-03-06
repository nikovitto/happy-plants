<template>
  <main class="main-wireframe">
    <app-header :back="true">
      <h1 slot="title">Add a new friend</h1>
    </app-header>

    <section>
      <form @submit.prevent="validateForm">
        <label for="register-name" class="form-label-group">
          <h2 class="required">What's your friends name?</h2>
          <span></span>
          <input required
            type="text"
            id="register-name"
            placeholder="Name"
            @change="getName" />
        </label>

        <label for="register-file" class="form-label-group">
          <h2>Upload photo</h2>
          <span>You can either select a photo from your gallery or take one now.</span>
          <file-upload
            name="register-file"
            @file-selected="getFile"
            @loading-file="handleLoadingState" />
        </label>

        <button :disabled="isUploadingFile">
          Add plant
        </button>
      </form>
    </section>
  </main>
</template>

<script>
  import { mapActions } from 'vuex'
  import AppHeader from '@/components/AppHeader'
  import FileUpload from '@/components/FileUpload'
  import getDefaultStructure from '@/utils/get-default-structure'

  export default {
    name: 'RegisterPlant',

    components: {
      'app-header': AppHeader,
      'file-upload': FileUpload
    },

    data () {
      return {
        name: '',
        blob: undefined,
        isUploadingFile: false
      }
    },

    methods: {
      ...mapActions([
        'addPlant'
      ]),
      handleLoadingState ({ loading }) {
        this.isUploadingFile = loading
      },
      getFile (data) {
        this.blob = data.blob
      },
      getName (event) {
        if (!event.target.value) return
        this.name = event.target.value
      },
      validateForm () {
        const config = {
          ...getDefaultStructure(),
          blob: this.blob,
          name: this.name
        }
        this.addPlant(config).then(guid =>
          this.$router.push(`/plant/${guid}`))
      }
    }
  }
</script>

<style lang="scss" scoped>
  main {
    background: var(--background-secondary);

    section {
      padding: var(--base-gap) 0;
      line-height: 150%;
    }
  }

  form {
    position: relative;
    z-index: 1;
    padding: 0 var(--base-gap);
    width: 100%;

    input {
      width: 100%;
    }
  }

  .form-order {
    display: flex;
    background: white;
    padding: 0;
    margin-bottom: var(--base-gap);
    border: none;
    border-radius: var(--border-radius);
    box-shadow: 4px 4px 0 rgba(0, 0, 0, 0.12);
    width: 100%;
  }

  .form-label-group {
    display: block;
    width: 100%;
    margin-bottom: calc(var(--base-gap) * 2);
  }

  label h2 {
    margin-bottom: calc(var(--base-gap) / 2);

    &.required::after {
      color: var(--brand-blue);
      content: " *";
      font-size: var(--text-size-small);
    }
  }

  label span {
    display: block;
    color: var(--text-color-secondary);
    font-size: var(--text-size-small);
    margin-bottom: calc(var(--base-gap) / 2);
    padding: 0 1px;
  }
</style>
