<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT POST</h5>
          <hr>
          <b-form @submit="update">
            <b-form-group label="Provinsi">
              <b-form-input type="text" v-model="provinsi.provinsi" :class="{ 'is-invalid': validation.provinsi }"
                placeholder="masukkan provinsi">
              </b-form-input>
              <div v-if="validation.provinsi" class="mt-2">
                <b-alert show variant="danger">{{ validation.provinsi[0] }}</b-alert>
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
        provinsi: {
          provinsi: ''
        },
        //state validation
        validation: []
      }
    },

    mounted() {
console.log(this.$route.params.id);
      //get data post by ID
      this.$axios.get(`/api/provinsi/${this.$route.params.id}`)
        .then(response => {
            this.provinsi.provinsi   = response.data.data.provinsi
        })
    },

    methods: {

      async update(e) {
        e.preventDefault()

        //send data ke Rest API untuk update
        await this.$axios.put(`/api/provinsi/${this.$route.params.id}`, {

            //data yang dikirim
            provinsi: this.provinsi.provinsi

          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'provinsi'
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