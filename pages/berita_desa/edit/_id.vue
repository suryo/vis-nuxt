<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT POST</h5>
          <hr>
          <b-form @submit="update">
             <b-form-group label="Desa">
            <b-form-select v-model="berita_desa.id_desa" :options="desa"  value-field="id" text-field="desa"></b-form-select>
            <div v-if="validation.berita_desa" class="mt-2">
                <b-alert show variant="danger">{{ validation.berita_desa[0] }}</b-alert>
              </div>
             </b-form-group>

            <b-form-group label="Judul">
              <b-form-input type="text" v-model="berita_desa.judul" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Judul">
              </b-form-input>
              <div v-if="validation.berita_desa" class="mt-2">
                 <b-alert show variant="danger">{{ validation.berita_desa[1] }}</b-alert>
              </div>
            </b-form-group>

<b-form-group label="Tanggal">
              <b-form-input type="date" v-model="berita_desa.tanggal" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Tanggal">
              </b-form-input>
              <div v-if="validation.berita_desa" class="mt-2">
                 <b-alert show variant="danger">{{ validation.berita_desa[2] }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Berita">
              <b-form-input type="text" v-model="berita_desa.berita" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Berita">
              </b-form-input>
              <div v-if="validation.berita_desa" class="mt-2">
                 <b-alert show variant="danger">{{ validation.berita_desa[3] }}</b-alert>
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
        provinsi:[],
        berita_desa: {
          id_desa: '',
          judul: '',
          tanggal: '',
          berita: '',
        },
        //state validation
        validation: []
      }
    },

    mounted() {
console.log(this.$route.params.id);
      //get data post by ID
      this.$axios.get(`/api/berita_desa/${this.$route.params.id}`)
        .then(response => {
            this.berita_desa.id_desa   = response.data.data.id_desa,
            this.berita_desa.judul   = response.data.data.judul,
            this.berita_desa.tanggal   = response.data.data.tanggal,
            this.berita_desa.berita   = response.data.data.berita,

            this.$axios.get('/api/desa')
            .then(response => {
              //assign response ke state "desa"
              this.desa = response.data.data;
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
        await this.$axios.put(`/api/berita_desa/${this.$route.params.id}`, {

            //data yang dikirim
            id_desa:   this.berita_desa.id_desa,
            judul:   this.berita_desa.judul,
            tanggal:   this.berita_desa.tanggal,
            berita:   this.berita_desa.berita

          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'berita_desa'
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