<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH PROVINSI</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Provinsi">
              <b-form-input type="text" v-model="provinsi.provinsi" :class="{ 'is-invalid': validation.title }"
                placeholder="masukkan provinsi">
              </b-form-input>
              <div v-if="validation.provinsi" class="mt-2">
                <b-alert show variant="danger">{{ validation.provinsi[0] }}</b-alert>
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
        provinsi: {
          provinsi: ''
        },
        //state validation
        validation: []
      }
    },

    methods: {
      
      //method "store"
      async store(e) {
        e.preventDefault()

        //send data ke Rest API
        await this.$axios.post('/api/provinsi', {

            //data yang dikirim ke server
            provinsi:   this.provinsi.provinsi
            
          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'provinsi'
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