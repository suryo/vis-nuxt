<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA Kota</h5>
          <hr />
          <b-button :to="{ name: 'kota-create' }" variant="primary" class="mb-3"
            >TAMBAH</b-button
          >
          <b-table
            striped
            bordered
            hover
            :items="kota"
            :fields="fields"
            show-empty
          >
            <template v-slot:cell(actions)="row">
              <b-button
                :to="{ name: 'kota-edit-id', params: { id: row.item.id } }"
                variant="warning"
                size="sm"
                >EDIT</b-button
              >
              <b-button variant="danger" size="sm" @click="deleteKota(row)"
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
      fields: ["kota", "actions"],
      //kota data
      kota: []
    };
  },

  mounted() {
    console.log("asik");
    //fething ke Rest API
    this.$axios
      .get("/api/kota")
      .then(response => {
        //assign response ke state "kota"
        this.kota = response.data.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },

  methods: {
    async deleteKota(row) {
      //delete data post by ID
      await this.$axios.delete(`/api/kota/${row.item.id}`).then(() => {
        //remove item array by index
        this.kota.splice(row.index, 1);
      });
    }
  }
};
</script>

<style></style>
