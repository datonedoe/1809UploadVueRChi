<template>
  <form @submit.prevent="sendFile" enctype="multipart/form-data">
    <div class="field">

      <div class="file is-boxed is-primary">
        <label class="file-label">
          <input ref="file" type="file" @change="selectFile" class="file-input" />
          <span class="file-cta">
            <div class="file-icon">
              <i class="fas fa-upload"></i>
            </div>
            <span class="file-label">
              Choose a file...
            </span>
          </span>

          <span v-if="file" class="file-name">{{file.name}}</span>
        </label>
      </div>
    </div>

    <div class="field">
      <button class="button is-info">Send</button>
    </div>
  </form>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'SimpleUpload',
    data() {
      return {
        file: ''
      }
    },
    methods: {
      selectFile() {
        this.file = this.$refs.file.files[0]
      },

      async sendFile() {
        const formData = new FormData();
        formData.append('file', this.file);

        try {
          await axios.post('/upload', (formData))
        } catch (error) {
          console.log(err)
        }
      }
    }
  }
</script>