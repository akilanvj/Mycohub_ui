<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card class="card-plain">
            <template slot="header">
              <h4 class="card-title">All Process Sequence Files - FASTQ.GZ to FASTA</h4>
              <p class="card-category"></p>
            </template>
            <div class="table-responsive">
              <l-table class="table-hover" :columns="table1.columns" :data="table1.data">
              </l-table>
            </div>
          </card>
        </div>

      </div>
    </div>
  </div>
</template>
<script>
import LTable from 'src/components/Table.vue'
import Card from 'src/components/Cards/Card.vue'
let tableColumns = ['Id', 'uploadeddate','Folder', 'Filename', 'FileSize'];
let tableData = [];
export default {
  components: {
    LTable,
    Card
  },
  data() {
    return {
      tableColumns: ['Id', 'Uploadeddate','Folder', 'Filename', 'FileSize'],
      tableData: [],
      table1: ''
    }
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch('http://127.0.0.1:8000/files/all')
        .then(response => response.json())
        .then(data => {
          this.tableData = data;
          this.table1 = {
            columns: [...tableColumns],
            data: [...this.tableData]
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
  },
  mounted() {
    this.fetchData();
  },
}

</script>
<style></style>
