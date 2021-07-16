<template>
    <q-page padding class="bg-grey-2">
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div  class="col-aotu">
            <div class="left blue"> </div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-black" >
              <div class="text-h5"> Input Barang </div>
              <div> Data Barang Yang Diinput</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
     <q-card flat class="warna">
      <q-card-section class="row">
        <q-form
         @submit="onSubmit()"
          class="q-gutter-md col-md-6 col-xs-12"
        >
          <q-input
                filled
                v-model="form.namaBarang"
                label="Nama Barang "
                :rules="[ val => val && val.length > 0 || 'Mohon Isi Nama Barang']"
            />

             <q-input
                filled
                v-model="form.harga"
                type="number"
                label="harga"
                :rules="[ val => val > 0 || 'Mohon Isi Harga']"
            />

            <q-select
              standout="bg-primary text-white"
              v-model="form.typeBarang"
              :options="optiontypebarang"
              label="Type Barang"
              />

            <q-input
              v-model="form.deskripsi"
              filled
              type="textarea"
              label="Deskripsi"
            />

          <q-file accept=".jpg, image/*" color="teal" filled v-model="image" label="Upload Gambar">
            <template v-slot:prepend>
              <q-icon name="cloud_upload" />
            </template>
          </q-file>

            <div>
                <q-btn label="Submit" type="submit" color="primary"/>
                <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
            </div>
            </q-form>
      </q-card-section>
    </q-card>
</q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        namaBarang: null,
        harga: 0,
        typeBarang: null,
        rating: 0,
        deskripsi: null
      },
      optiontypebarang: [
        'sepatu',
        'raket',
        'kaoskai',
        'shuttlecock'
      ],
      image: null
    }
  },
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('Barang/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'datadvd' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
