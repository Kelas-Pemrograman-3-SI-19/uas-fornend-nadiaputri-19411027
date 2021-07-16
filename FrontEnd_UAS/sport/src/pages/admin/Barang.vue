<template>
  <q-page padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
             <q-banner inline-actions class="text-blue-grey-14">
              <div class="text-h6">Data kamera </div>
              <div>Data Katalog kamera Anda</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card flat>
       <q-table
      :rows="data"
      flat
      :columns="columns"
      row-key="name"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="namaBarang" :props="props">
            {{ props.row.namaBarang }}
          </q-td>
          <q-td key="harga" :props="props">
              Rp. {{ props.row.harga }},-
          </q-td>
          <q-td key="typeBarang" :props="props">
             {{ props.row.typeBarang }}
          </q-td>
          <q-td key="deskripsi" :props="props">
              <div class="ellipsis" style="max-width: 200px">
                {{ props.row.deskripsi }}
              </div>
          </q-td>
          <q-td key="gambar" :props="props">
              <q-img
                :src="`${$baseImageURL}/${props.row.image}`"
                spinner-color="white"
              />
          </q-td>
          <q-td key="aksi" :props="props">
            <div class="row q-gutter-md">
              <q-btn :to="{ name: 'formateditdvd', params: { id: props.row._id}}" label="Edit" icon="edit" color="warning" unelevated/>
              <q-btn  @click="deletekamera(props.row._id)" label="Hapus" icon="delete" color="negative" unelevated/>
            </div>
          </q-td>
        </q-tr>
      </template>
    </q-table>
    </q-card>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      columns: [
        { name: 'namaBarang', align: 'left', label: 'namabarang', field: 'namaBarang', sortable: true },
        { name: 'harga', align: 'left', label: 'Harga', field: 'harga', sortable: true },
        { name: 'typeBarang', align: 'left', label: 'Type Barang', field: 'typeBarang', sortable: true },
        { name: 'deskripsi', align: 'left', label: 'Deskripsi', field: 'deskripsi', sortable: true },
        { name: 'gambar', align: 'left', label: 'Gambar', field: 'gambar' },
        { name: 'aksi', align: 'left', label: 'Aksi', field: 'aksi' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('Barang/tampilbarang')
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.showNotif(res.data.pesan, 'negative')
          }
        })
    },
    deletekamera (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin ?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete(`Barang/delete/${id}`)
          .then(res => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
    }
  }
}
</script>
<style scoped>
.left {
  width: 3px;
  height: 100%;
  background-color: aqua;
}
</style>
