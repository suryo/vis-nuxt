<template>
  <div>

    <div>
      <b-navbar toggleable="lg" type="light" variant="light" class="shadow-md" fixed="top">
        <b-navbar-brand to="/">NUXT CRUD</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item to="/provinsi">Provinsi</b-nav-item>
            <b-nav-item to="/kabupaten">Kabupaten</b-nav-item>
            <b-nav-item to="/kota">Kota</b-nav-item>
            <b-nav-item to="/kecamatan">Kecamatan</b-nav-item>
            <b-nav-item to="/desa">Desa</b-nav-item>
            <b-nav-item to="/berita_desa">Berita Desa</b-nav-item>
            <b-nav-item to="/lembagadesa">Lembaga Desa</b-nav-item>
            <b-nav-item to="/jenispotensidesa">Jenis Potensi Desa</b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>

    <Nuxt />
  </div>
</template>

<style>
body{
  background-color: lightgray;
}
.mt-5 {
  margin-top: 100px!important;
}
</style>