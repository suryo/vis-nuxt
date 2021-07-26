<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT POST</h5>
          <hr>
          <b-form @submit="update">

            <b-form-group label="Desa">
            <b-form-select v-model="lembagadesa.id_desa" :options="desa"  value-field="id" text-field="nama_desa"></b-form-select>
                <div v-if="validation.lembagadesa" class="mt-2">
                  <b-alert show variant="danger">{{ validation.lembagadesa[0] }}</b-alert>
                </div>
            </b-form-group>

            <b-form-group label="Jenis Lembaga">
            <b-form-select v-model="lembagadesa.id_jenis_lembaga" :options="jenislembaga"  value-field="id" text-field="nama_lembaga"></b-form-select>
                <div v-if="validation.lembagadesa" class="mt-2">
                  <b-alert show variant="danger">{{ validation.lembagadesa[1] }}</b-alert>
                </div>
            </b-form-group>

            <b-form-group label="Lembaga Desa">
              <b-form-input type="text" v-model="lembagadesa.lembaga_desa" :class="{ 'is-invalid': validation.lembagadesa }"
                placeholder="masukkan lembaga desa">
              </b-form-input>
              <div v-if="validation.lembagadesa" class="mt-2">
                <b-alert show variant="danger">{{ validation.lembagadesa[3] }}</b-alert>
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
        jenislembaga:[],
        desa:[],
        lembagadesa: {
          id_desa: '',
          id_jenis_lembaga: '',
          lembaga_desa: '',
        },
        //state validation
        validation: []
      }
    },

    mounted() {
    console.log(this.$route.params.id);
      //get data post by ID
      this.$axios.get(`/api/lembagadesa/${this.$route.params.id}`)
        .then(response => {
             this.lembagadesa.id_desa   = response.data.data.id_desa,
            this.lembagadesa.id_provinsi   = response.data.data.id_provinsi,
            this.lembagadesa.lembagadesa   = response.data.data.lembagadesa,

            this.$axios.get('/api/desa')
            .then(response => {
            //assign response ke state "desa"
            this.desa = response.data.data;
            })
            .catch(error => {
            console.log("error gan")
            })

            this.$axios.get('/api/jenislembagadesa')
            .then(response => {
            //assign response ke state "jenislembaga"
            this.jenislembaga = response.data.data;
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
        await this.$axios.put(`/api/lembagadesa/${this.$route.params.id}`, {

            //data yang dikirim
            id_desa:   this.lembagadesa.id_desa,
            id_provinsi:   this.lembagadesa.id_provinsi,
            lembagadesa:   this.lembagadesa.lembagadesa

          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'lembagadesa'
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