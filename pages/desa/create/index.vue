<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Tambah Data Desa</h5>
          <hr />
          <b-form @submit="store">
            <b-form-group label="Provinsi">
              <b-form-select
                v-model="desa.id_provinsi"
                :options="provinsi"
                value-field="id"
                text-field="provinsi"
              ></b-form-select>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[0]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kabupaten">
              <b-form-select
                v-model="desa.id_kabupaten"
                :options="kabupaten"
                value-field="id"
                text-field="kabupaten"
              ></b-form-select>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[1]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kota">
              <b-form-select
                v-model="desa.id_kota"
                :options="kota"
                value-field="id"
                text-field="Kota"
              ></b-form-select>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[2]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kecamatan">
              <b-form-select
                v-model="desa.id_kecamatan"
                :options="kecamatan"
                value-field="id"
                text-field="Kecamatan"
              ></b-form-select>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[3]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Nama Desa">
              <b-form-input
                type="text"
                v-model="desa.nama_desa"
                :class="{ 'is-invalid': validation.title }"
                placeholder="Nama Desa"
              >
              </b-form-input>
              <div v-if="validation.kabupaten" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[4]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Alamat Desa">
              <b-form-input
                type="text"
                v-model="desa.alamat_lengkap"
                :class="{ 'is-invalid': validation.title }"
                placeholder="Alamat Lengkap Desa"
              >
              </b-form-input>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[5]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Deskripsi">
              <b-form-input
                type="text"
                v-model="desa.deskripsi"
                :class="{ 'is-invalid': validation.title }"
                placeholder="Deskripsi Desa"
              >
              </b-form-input>
              <div v-if="validation.desa" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.desa[6]
                }}</b-alert>
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
      provinsi: [],
      kabupaten: [],
      kota: [],
      kecamatan: [],
      desa: {
        id_provinsi: "",
        id_kabupaten: "",
        id_kota: "",
        id_kecamatan: "",
        nama_desa: "",
        alamat_lengkap: "",
        deskripsi: ""
      },
      //state validation
      validation: [],
      selected: null
    };
  },

  mounted() {
  this.loading = true;
  Promise.all([this.getProvinsi(),this.getKabupaten()])
    .then(values => {
        console.log("promise");
        console.log(values[0]);
        this.provinsi = values[0];
        this.kabupaten = values[1];
        this.loading = false;
    }).catch(errors => {
        this.errormsg = error.response.data.message;
        this.loading = false;
    })
    //fething ke Rest API
    // this.$axios
    //   .get("/api/provinsi")
    //   .then(response => {
        //assign response ke state "provinsi"
      //   this.provinsi = response.data.data;
      // })
      // .catch(error => {
      //   console.log(error.response.data);
      // });

  },

  methods: {
    //method "store"
    async store(e) {
      e.preventDefault();
      //send data ke Rest API
      await this.$axios
        .post("/api/desa", {
          //data yang dikirim ke server
          id_provinsi: this.desa.id_provinsi,
          id_kota: this.desa.id_kota,
          id_kabupaten: this.desa.id_kabupaten,
          id_kecamatan: this.desa.id_kecamatan,
          nama_desa: this.desa.nama_desa,
          alamat_lengkap: this.desa.alamat_lengkap,
          deskripsi: this.desa.deskripsi
        })
        .then(() => {
          //redirect ke route "post"
          this.$router.push({
            name: "desa"
          });
        })
        .catch(error => {
          //assign validation
          this.validation = error.response.data;
        });
    },

    async getProvinsi() {
      const response = await this.$axios.get("/api/provinsi");
      return response.data.data;
    },
    async getKota() {
      const response = await this.$axios.get("/api/kota");
      return response.data.data;
    },
    async getKabupaten() {
      const response = await this.$axios.get("/api/kabupaten");
      return response.data.data;
    },
    async getKecamatan() {
      const response = await this.$axios.get("/api/kecamatan");
      return response.data.data;
    }
  },

  
};
</script>

<style></style>
