<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH Kota</h5>
          <hr />
          <b-form @submit="store">
            <b-form-group label="Provinsi">
              <b-form-select
                v-model="kota.id_provinsi"
                :options="provinsi"
                value-field="id"
                text-field="provinsi"
              ></b-form-select>
              <div v-if="validation.kota" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kota[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Kota">
              <b-form-input
                type="text"
                v-model="kota.kota"
                :class="{ 'is-invalid': validation.title }"
                placeholder="Masukkan Nama Kota"
              >
              </b-form-input>
              <div v-if="validation.kota" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kota[1]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-button type="submit" variant="primary">Save</b-button>
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
      kota: {
        id_provinsi: "",
        kota: ""
      },
      //state validation
      validation: [],
      selected: null
    };
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
        .post("/api/kota", {
          //data yang dikirim ke server
          id_provinsi: this.kota.id_provinsi,
          kota: this.kota.kota
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
