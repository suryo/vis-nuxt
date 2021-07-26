<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA BERITA DESA</h5>
          <hr>
          <b-button :to="{name: 'berita_desa-create'}" variant="primary" class="mb-3">TAMBAH</b-button>
          <b-table striped bordered hover :items="berita_desa" :fields="fields" show-empty>
               <template v-slot:cell(actions)="row">
                <b-button :to="{name: 'berita_desa-edit-id',params: {id: row.item.id}}" variant="warning" size="sm">EDIT</b-button>
                <b-button variant="danger" size="sm" @click="deleteberita_desa(row)">DELETE</b-button>
              </template>
          </b-table>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
    data() {
      return {
        //header table  
        fields: ['id_desa','judul','tanggal','berita', 'actions'],
        //berita_desa data
        berita_desa: [],
      }
    },

    mounted() {
      console.log("asik");
      //fething ke Rest API 
      this.$axios.get('/api/beritadesa')
        .then(response => {
          //assign response ke state "berita_desa"
          this.berita_desa = response.data.data
console.log(this.berita_desa);
        })
        .catch(error => {
          console.log(error.response.data)
        })
    },



  methods: {

       async deleteberita_desa(row) {
        
        //delete data post by ID
        await this.$axios.delete(`/api/beritadesa/${row.item.id}`)
          .then(() => {

            //remove item array by index
            this.berita_desa.splice(row.index, 1);

          })

      }
    }

  }
</script>

<style>

</style>