<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Tambah Kecamatan</h5>
          <hr />
          <b-form @submit="store">
            <b-form-group label="Provinsi">
              <b-form-select
                v-model="kecamatan.id_provinsi"
                :options="provinsi"
                value-field="id"
                text-field="provinsi"
              ></b-form-select>
              <div v-if="validation.kecamatan" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kecamatan[0]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kabupaten">
              <b-form-select
                v-model="kecamatan.id_kabupaten"
                :options="kabupaten"
                value-field="id"
                text-field="kabupaten"
              ></b-form-select>
              <div v-if="validation.kecamatan" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kecamatan[0]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kota">
              <b-form-select
                v-model="kecamatan.id_kota"
                :options="kota"
                value-field="id"
                text-field="kota"
              ></b-form-select>
              <div v-if="validation.kecamatan" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kecamatan[0]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-form-group label="Kecamatan">
              <b-form-input
                type="text"
                v-model="kecamatan.kecamatan"
                :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan kecamatan"
              >
              </b-form-input>
              <div v-if="validation.kecamatan" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kecamatan[1]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-button type="submit" variant="primary">Simpan</b-button>
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
      provinsi: {},
      kota: {
        selectedProvinsi: ""
      },
      kabupaten: {
        selectedProvinsi: ""
      },
      kecamatan: {
        id_provinsi: "",
        id_kabupaten: "",
        id_kota: "",
        kecamatan: ""
      },
      //state validation
      validation: [],
      selected: null
    };
  },

  watch: {
    selectedProvinsi: function(value) {
      axios
        .get("/api/kabupaten?id_provinsi=" + this.id_provinsi)
        .then(response => {
          this.kabupaten = response.data.data;
        })
        .catch(error => {
          console.log(error.response.data);
        });
    }
  },

  mounted() {
    //fething ke Rest API
    this.$axios
      .get("/api/provinsi")
      .then(response => {
        //assign response ke state "provinsi"
        this.provinsi = response.data.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },

  methods: {
    //method "store"
    async store(e) {
      e.preventDefault();
      //send data ke Rest API
      await this.$axios
        .post("/api/kecamatan", {
          //data yang dikirim ke server
          id_provinsi: this.kecamatan.id_provinsi,
          id_kabupaten: this.kecamatan.id_kabupaten,
          id_kota: this.kecamatan.id_kota,
          kecamatan: this.kecamatan.kecamatan
        })
        .then(() => {
          //redirect ke route "post"
          this.$router.push({
            name: "kota"
          });
        })
        .catch(error => {
          //assign validation
          this.validation = error.response.data;
        });
    }
  }
};
</script>

<style></style>
