<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Tambah Potensi Desa</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Nama Potensi Desa">
              <b-form-input type="text" v-model="jenispotensidesa.nama_potensi" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Nama Potensi">
              </b-form-input>
              <div v-if="validation.nama_potensi" class="mt-2">
                 <b-alert show variant="danger">{{ validation.nama_potensi[1] }}</b-alert>
              </div>
            </b-form-group>
          
            <b-button type="submit" variant="primary">SIMPAN</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {

    data() {
      return {
        //state post
        jenispotensidesa: {
          nama_potensi: '',
        },
        //state validation
        validation: [],
        selected: null,
      }
    },

    mounted() {
      //fething ke Rest API 
      this.$axios.get('/api/provinsi')
        .then(response => {
          //assign response ke state "provinsi"
          this.provinsi = response.data.data;
        })
        .catch(error => {
          console.log(error.response.data)
        })
    },

    methods: {
      
      //method "store"
      async store(e) {
        e.preventDefault()
        //send data ke Rest API
        await this.$axios.post('/api/jenispotensidesa', {
            //data yang dikirim ke server
            jenispotensidesa:   this.jenispotensidesa.nama_potensi
          })
          .then(() => {
            //redirect ke route "post"
            this.$router.push({
              name: 'jenispotensidesa'
            })
          })
          .catch(error => {
            //assign validation  
            this.validation = error.response.data
          })
      }
    }

  }
</script>

<style>

</style>