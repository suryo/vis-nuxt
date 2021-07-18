<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH KABUPATEN</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Provinsi">
            <b-form-select v-model="kabupaten.id_provinsi" :options="provinsi"  value-field="id" text-field="provinsi"></b-form-select>
            <div v-if="validation.kabupaten" class="mt-2">
                <b-alert show variant="danger">{{ validation.kabupaten[0] }}</b-alert>
              </div>
             </b-form-group>
            <b-form-group label="Kabupaten">
              <b-form-input type="text" v-model="kabupaten.kabupaten" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Kabupaten">
              </b-form-input>
              <div v-if="validation.kabupaten" class="mt-2">
                 <b-alert show variant="danger">{{ validation.kabupaten[1] }}</b-alert>
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
        provinsi:[],
        kabupaten: {
          id_provinsi: '',
          kabupaten: '',
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
        await this.$axios.post('/api/kabupaten', {
            //data yang dikirim ke server
            id_provinsi:   this.kabupaten.id_provinsi,
            kabupaten:   this.kabupaten.kabupaten
          })
          .then(() => {
            //redirect ke route "post"
            this.$router.push({
              name: 'kabupaten'
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