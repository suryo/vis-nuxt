<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA LEMBAGA DESA</h5>
          <hr>
          <b-button :to="{name: 'lembagadesa-create'}" variant="primary" class="mb-3">TAMBAH</b-button>
          <b-table striped bordered hover :items="lembagadesa" :fields="fields" show-empty>
               <template v-slot:cell(actions)="row">
                <b-button :to="{name: 'lembagadesa-edit-id',params: {id: row.item.id}}" variant="warning" size="sm">EDIT</b-button>
                <b-button variant="danger" size="sm" @click="deletelembagadesa(row)">DELETE</b-button>
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
        fields: ['Lembaga Desa', 'Desa', 'Jenis Lembaga', 'actions'],
        //lembagadesa data
        lembagadesa: [],
      }
    },

    mounted() {
      console.log("asik");
      //fething ke Rest API 
      this.$axios.get('/api/lembagadesa')
        .then(response => {
          //assign response ke state "lembagadesa"
          this.lembagadesa = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
    },



  methods: {

       async deletelembagadesa(row) {
        
        //delete data post by ID
        await this.$axios.delete(`/api/lembagadesa/${row.item.id}`)
          .then(() => {

            //remove item array by index
            this.lembagadesa.splice(row.index, 1);

          })

      }
    }

  }
</script>

<style>

</style>