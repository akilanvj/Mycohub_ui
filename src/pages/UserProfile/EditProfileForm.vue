<template>
  <card>
    <h4 slot="header" class="card-title">Upload File</h4>
    <div class="row justify-content-center">
    </div>
    <form enctype="multipart/form-data" ref="form">
      <div class="row">
        <div class="col-md-3">
          <base-input type="text" label="File Name" placeholder="File Name" v-model="user.file_name">
          </base-input>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4">
          <base-input type="file" label="File Upload" ref="file" placeholder="Upload the file" v-model="user.file"
            @change="changeFile($event)">
          </base-input>
          <div v-if="errors.length > 0">
            <div class="file-upload__error" v-for="(error, index) in errors" :key="index">
              <span>{{ error }}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="handleFiles()">
          Upload File
        </button>
      </div>
      <div class="clearfix"></div>
    </form>
  </card>
</template>
<script>
import Card from 'src/components/Cards/Card.vue'

export default {
  components: {
    Card
  },
  data() {
    return {
      type: ['', 'info', 'success', 'warning', 'danger'],
      errors: [],
      user: {
        file: null
      },
      notifications: {
        topCenter: true
      },
      isLoading: false,
      uploadReady: true,
      file: {
        name: "",
        size: 0,
        type: "",
        fileExtention: "",
        url: "",
        isImage: false,
        isUploaded: false,
      },
    }
  },
  props: {
  },
  methods: {
    isFileSizeValid(fileSize) {
      if (fileSize <= this.maxSize) {
        console.log("File size is valid");
      } else {
        this.errors.push(`File size should be less than ${this.maxSize} MB`);
      }
    },
    notifyVue() {
      alert('File Uploaded Successfully.');
    },
    isFileTypeValid(fileExtention) {
      if (this.accept.split(",").includes(fileExtention)) {
        console.log("File type is valid");
      } else {
        this.errors.push(`File type should be ${this.accept}`);
      }
    },
    isFileValid(file) {
      this.isFileSizeValid(Math.round((file.size / 1024 / 1024) * 100) / 100);
      this.isFileTypeValid(file.name.split(".").pop());
      if (this.errors.length === 0) {
        return true;
      } else {
        return false;
      }
    },
    handleFiles() {
      const formData = new FormData(this.$refs.form);
      formData.append('file', this.user.file);
      formData.append('file_name', this.user.file_name);
      formData.append('redirect', "follow");

      console.log(formData);

      //let headers = new Headers();
      //headers.append('Content-Type', 'multipart/form-data');
      //headers.append('Content-Type', `multipart/form-data; boundary="yet another boundary"`);
      fetch('http://127.0.0.1:8000/upload/', {
        method: 'POST',
        //headers: headers,
        body: formData
      })
        .then(response => response.json())
        .then(data => {
          console.log('Success:', data);
        })
        .catch(error => {
          console.error('Error:', error);

        });
      this.notifyVue();
      // this.$notifications.notify(
      //   {
      //     message: `<span>File Uploaded Successfully</span>`,
      //     icon: 'nc-icon nc-app',
      //     horizontalAlign: 'center',
      //     verticalAlign: 'top',
      //     type: 'success'
      //   });
    },
    changeFile(e) {
      let fileNameOnly = '';
      console.log(e.target.files[0]);
      // Check if file is selected
      if (e.target.files && e.target.files[0]) {
        // Get uploaded file
        const file = e.target.files[0],
          // Get file size
          fileSize = Math.round((file.size / 1024 / 1024) * 100) / 100,
          // Get file extension
          fileExtention = file.name.split(".").pop(),
          // Get file name
          fileName = file.name.split(".").shift(),
          // Check if file is an image
          isUploadFileTypes = ["fastq", "fasta", "fastq.gz", "fasta.gz", "gz"].includes(fileExtention);
        // Print to console
        console.log(fileSize, fileExtention, fileNameOnly, isUploadFileTypes);
      }

      this.user.file = e.target.files[0];
      console.log('Changing the file name -> []');
      console.log(this.user.file);
    }
  }
}

</script>
<style></style>
