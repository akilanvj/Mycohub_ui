<template>
  <card>
    <h4 slot="header" class="card-title">Upload File</h4>
    <form>
      <div class="row">
        <div class="col-md-3">
          <base-input type="text" label="File Name" placeholder="File Name" v-model="user.file_name">
          </base-input>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4">
          <base-input type="file" label="File Upload" placeholder="Upload the file" v-model="user.file">
          </base-input>
        </div>
      </div>

      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="handleFiles">
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
      user: {
        file: ''
      }
    }
  },
  methods: {
    updateProfile() {
      alert('Your data: ' + JSON.stringify(this.user))
    },
    handleFiles() {
      console.log('Uploaded File : ' + JSON.stringify(this.user));
    }
  }
}

</script>
<style></style>
