<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA Desa</h5>
          <hr />
          <b-button :to="{ name: 'desa-create' }" variant="primary" class="mb-3"
            >TAMBAH</b-button
          >
          <b-table
            striped
            bordered
            hover
            :items="desa"
            :fields="fields"
            show-empty
          >
            <template v-slot:cell(actions)="row">
              <b-button
                :to="{ name: 'desa-edit-id', params: { id: row.item.id } }"
                variant="warning"
                size="sm"
                >EDIT</b-button
              >
              <b-button variant="danger" size="sm" @click="deleteDesa(row)"
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
      fields: ["desa", "actions"],
      //desa data
      desa: []
    };
  },

  mounted() {
    console.log("asik");
    //fething ke Rest API
    this.$axios
      .get("/api/desa")
      .then(response => {
        //assign response ke state "desa"
        this.desa = response.data.data;
      })
      .catch(error => {
        console.log(error.response.data);
      });
  },

  methods: {
    async deleteDesa(row) {
      //delete data post by ID
      await this.$axios.delete(`/api/desa/${row.item.id}`).then(() => {
        //remove item array by index
        this.desa.splice(row.index, 1);
      });
    }
  }
};
</script>

<style></style>
