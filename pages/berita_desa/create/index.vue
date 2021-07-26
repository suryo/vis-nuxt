<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH Berita Desa</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Desa">
            <b-form-select v-model="berita_desa.id_desa" :options="desa"  value-field="id" text-field="nama_desa"></b-form-select>
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

              <b-form-group label="tanggal">
              <b-form-input type="date" v-model="berita_desa.tanggal" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Tanggal">
              </b-form-input>
              <div v-if="validation.berita_desa" class="mt-2">
                 <b-alert show variant="danger">{{ validation.berita_desa[2] }}</b-alert>
              </div>
              </b-form-group>

              <b-form-group label="Berita">
              <b-form-input type="textarea" v-model="berita_desa.berita" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Berita">
              </b-form-input>
              <div v-if="validation.berita_desa" class="mt-2">
                 <b-alert show variant="danger">{{ validation.berita_desa[3] }}</b-alert>
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
        desa:[],
        berita_desa: {
          id_desa: '',
          judul: '',
          tanggal: '',
          berita: '',
        },
        //state validation
        validation: [],
        selected: null,
      }
    },

    mounted() {
      //fething ke Rest API 
      console.log("masuk");
      this.$axios.get('/api/desa')
        .then(response => {
          //assign response ke state "desa"
          this.desa = response.data.data;
          console.log(this.desa);
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
        await this.$axios.post('/api/beritadesa', {
          
            //data yang dikirim ke server
            id_desa:   this.berita_desa.id_desa,
            judul:   this.berita_desa.judul,
            tanggal:   this.berita_desa.tanggal,
            berita:   this.berita_desa.berita
          })
          .then(() => {
            console.log("ayam");
            console.log(this.berita_desa.judul);
            //redirect ke route "post"
            this.$router.push({
              name: 'berita_desa'
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