<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA Kecamatan</h5>
          <hr />
          <b-button
            :to="{ name: 'kecamatan-create' }"
            variant="primary"
            class="mb-3"
            >TAMBAH</b-button
          >
          <b-table
            striped
            bordered
            hover
            :items="kecamatan"
            :fields="fields"
            show-empty
          >
            <template v-slot:cell(actions)="row">
              <b-button
                :to="{ name: 'kecamtan-edit-id', params: { id: row.item.id } }"
                variant="warning"
                size="sm"
                >EDIT</b-button
              >
              <b-button variant="danger" size="sm" @click="deleteKecamtan(row)"
                >DELETE</b-button
              >
            </template>
          </b-table>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      //header table
      fields: ["kecamatan", "actions"],
      //kota data
      kota: []
    };
  },

  mounted() {
    console.log("asik");
    //fething ke Rest API
    this.$axios
      .get("/api/kecatamatan")
      .then(response => {
        //assign response ke state "kota"
        this.kecamatan = response.data.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },

  methods: {
    async deleteKecamatan(row) {
      //delete data post by ID
      await this.$axios.delete(`/api/kecamatan/${row.item.id}`).then(() => {
        //remove item array by index
        this.kecamatan.splice(row.index, 1);
      });
    }
  }
};
</script>

<style></style>
