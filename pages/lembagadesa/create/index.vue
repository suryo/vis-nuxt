<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Tambah Lembaga Desa</h5>
          <hr>
          <b-form @submit="store">
           
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
              <b-form-input type="text" v-model="lembagadesa.lembaga_desa" :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Nama Jenis Lembaga Desa">
              </b-form-input>
              <div v-if="validation.lembagadesa" class="mt-2">
                <b-alert show variant="danger">{{ validation.lembagadesa[2] }}</b-alert>
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
        jenislembaga:[],
        desa:[],
        lembagadesa: {
          id_desa: '',
          id_jenis_lembaga: '',
          lembaga_desa: '',
        },
        //state validation
        validation: [],
        selected: null,
      }
    },

    mounted() {
      //fething ke Rest API 
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
    },

    methods: {
      
      //method "store"
      async store(e) {
        e.preventDefault()

        //send data ke Rest API
        await this.$axios.post('/api/lembagadesa', {

            //data yang dikirim ke server
            id_jenis_lembaga:   this.lembagadesa.id_jenis_lembaga,
            id_desa:       this.lembagadesa.id_desa,
            lembaga_desa:   this.lembagadesa.lembaga_desa
            
          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'lembagadesa'
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