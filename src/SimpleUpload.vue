<template>
  <form @submit.prevent="sendFile" enctype="multipart/form-data">
    <div
      v-if="message"
      :class="`message ${ error ? 'is-danger' : 'is-success'}`"
      >
      <div class="message-body">{{message}}</div>
    </div>
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
        file: '',
        message: '',
        error: false
      }
    },
    methods: {
      selectFile() {
        this.file = this.$refs.file.files[0];
        this.error = false;
        this.message = '';
      },

      async sendFile() {
        const formData = new FormData();
        formData.append('file', this.file);

        try {
          await axios.post('/upload', (formData));
          this.message = "File has been uploaded";
          this.file = "";
          this.error = false;
        } catch (error) {
          this.message = "Something went wrong";
          this.error = true;
        }
      }
    }
  }
</script>