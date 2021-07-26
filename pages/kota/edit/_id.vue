<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>Edit Kota</h5>
          <hr />
          <b-form @submit="update">
            <b-form-group label="Kota">
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
                placeholder="Masukkan Kota"
              >
              </b-form-input>
              <div v-if="validation.kota" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.kota[1]
                }}</b-alert>
              </div>
            </b-form-group>

            <b-button type="submit" variant="primary">Update</b-button>
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
      validation: []
    };
  },

  mounted() {
    console.log(this.$route.params.id);
    //get data post by ID
    this.$axios.get(`/api/kota/${this.$route.params.id}`).then(response => {
      (this.kota.id_provinsi = response.data.data.id_provinsi),
        (this.kota.kota = response.data.data.kota),
        this.$axios
          .get("/api/provinsi")
          .then(response => {
            //assign response ke state "kota"
            this.provinsi = response.data.data;
          })
          .catch(error => {
            console.log(error.response.data);
          });
    });
  },

  methods: {
    async update(e) {
      e.preventDefault();
      //send data ke Rest API untuk update
      await this.$axios
        .put(`/api/kota/${this.$route.params.id}`, {
          //data yang dikirim
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
          //assign error validasi
          this.validation = error.response.data;
        });
    }
  }
};
</script>

<style></style>
