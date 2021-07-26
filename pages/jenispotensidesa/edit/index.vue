<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT POST</h5>
          <hr>
          <b-form @submit="update">
            <b-form-group label="Nama Potensi">
              <b-form-input type="text" v-model="jenispotensidesa.nama_potensi" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Nama Potensi">
              </b-form-input>
              <div v-if="validation.nama_potensi" class="mt-2">
                 <b-alert show variant="danger">{{ validation.nama_potensi[1] }}</b-alert>
              </div>
            </b-form-group>



            
           
            <b-button type="submit" variant="primary">UPDATE</b-button>
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
        kabupaten: {
          nama_potensi: '',
        },
        //state validation
        validation: []
      }
    },

    mounted() {
console.log(this.$route.params.id);
      //get data post by ID
      this.$axios.get(`/api/jenispotensidesa/${this.$route.params.id}`)
        .then(response => {
            this.jenispotensidesa.nama_potensi   = response.data.data.nama_potensi,

            this.$axios.get('/api/provinsi')
            .then(response => {
              //assign response ke state "provinsi"
              this.provinsi = response.data.data;
            })
            .catch(error => {
              console.log(error.response.data)
            })
        })
    },

    methods: {

      async update(e) {
        e.preventDefault()

        //send data ke Rest API untuk update
        await this.$axios.put(`/api/jenispotensidesa/${this.$route.params.id}`, {

            //data yang dikirim
            jenispotensidesa:   this.jenispotensidesa.nama_potensi

          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'jenispotensidesa'
            })

          })
          .catch(error => {

            //assign error validasi  
            this.validation = error.response.data
          })
      }

    }

  }
</script>

<style>

</style>