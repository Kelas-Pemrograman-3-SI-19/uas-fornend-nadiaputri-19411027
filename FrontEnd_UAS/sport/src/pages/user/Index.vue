<template>
    <q-page padding>
      <q-carousel
      swipeable
      animated
      v-model="slide"
      autoplay
      ref="carousel"
      infinite
    >
      <q-carousel-slide :name="1" img-src="~assets/kevin.jpg" />
      <q-carousel-slide :name="2" img-src="~assets/lindan.jpg" />
      <q-carousel-slide :name="3" img-src="~assets/owi.jpg" />
      <q-carousel-slide :name="4" img-src="~assets/momota.jpg" />

      <template v-slot:control>
        <q-carousel-control
          position="top-right"
          :offset="[18, 18]"
          class="text-white rounded-borders"
          style="background: rgba(0, 0, 0, .3); padding: 4px 8px;"
        >
        </q-carousel-control>

        <q-carousel-control
          position="bottom-right"
          :offset="[18, 18]"
          class="q-gutter-xs"
        >
        </q-carousel-control>
      </template>
    </q-carousel>
    <div class="row q-col-gutter-md q-mt-md">
     <div class="col-md-3 col-xs-12" v-for="(Barang, i) in data" :key="i">
      <q-card>
      <q-img
       :src="`${$baseImageURL}/${Barang.image}`"
       :ratio="16/9"
      />

      <q-card-section>
        <q-btn
          fab
          color="red-9"
          icon="add_shopping_cart"
          class="absolute"
          unelevated
          style="top: 0; right: 12px; transform: translateY(-50%);"
        />

        <div class="row no-wrap items-center">
          <div class="col text-h6 ellipsis">
             {{ Barang.namaBarang }}
          </div>
        </div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <div class="text-subtitle1">
          Rp. {{ Barang.harga }},-
        </div>
        <div class="text-subtitle1">
         {{ Barang.typeBarang }}
        </div>
        <div @click="Barang.expanded = !Barang.expanded" class="text-caption text-grey cursor-pointer">
          Tampilkan Deskripsi
        </div>
        <q-slide-transition>
          <div v-show="Barang.expanded">
            <div class="text-grey text-caption">
              {{ Barang.deskripsi }}
            </div>
          </div>
        </q-slide-transition>
      </q-card-section>

      <q-card-actions>
        <q-btn unelevated @click="openDetail(Barang)" class="full-width" color="primary">
          Order Now
        </q-btn>
      </q-card-actions>
      </q-card>
      </div>
    </div>
     <q-dialog v-model="dialog" v-if="dialog" position="bottom">
      <q-card style="width: 500px">
        <q-card-section>
          <div class="text-h6">Detail Pemesanan</div>
        </q-card-section>
        <q-card-section style="max-height: 50vh;" class="scroll">
         {{ activeData.namaBarang }} - {{ activeData.harga }}
          <q-form class="q-mt-md">
            <q-input filled type="number" class="q-mb-md" v-model="jumlah" label="Masukkan Jumlah"/>
             Rp. {{ total }},-
            <q-file color="teal" class="q-mt-md" filled v-model="image" label="Label">
            <template v-slot:prepend>
              <q-icon name="cloud_upload" />
            </template>
          </q-file>
          </q-form>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat label="Batal" v-close-popup/>
          <q-btn color="primary"  @click="prosesTransaksi" label="proses"/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      slide: 1,
      stars: 4,
      dialog: false,
      image: null,
      data: [],
      activeData: null,
      jumlah: 1
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('Barang/getall')
        .then(res => {
          if (res.data.sukses) {
            this.data = res.data.data.map(Barang => {
              return Object.assign(Barang, {
                expanded: false
              })
            })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    },
    openDetail (data) {
      this.activeData = data
      this.dialog = true
    },
    prosesTransaksi () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify({
        idUser: this.$q.localStorage.getItem('dataUser')._id,
        idBarang: this.activeData._id,
        harga: this.activeData.harga,
        jumlah: this.jumlah,
        total: this.total
      }))
      this.$axios.post('order/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.dialog = false
            this.image = null
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  },
  computed: {
    total () {
      return this.activeData.harga * this.jumlah
    }
  }
}
</script>
