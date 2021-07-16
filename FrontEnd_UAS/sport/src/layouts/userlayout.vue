<template>
 <q-layout view="lHh LpR fFf">
     <q-toolbar uneleveted style="height: 70px" class="bg-primary text-white ">
      <div class="text-h5 q-ml-lg"> ULUL SPORT </div>

      <q-space />
        <div v-if="$q.screen.md || $q.screen.md || $q.screen.lg || $q.screen.xl">
              <q-btn :to="{ name: 'HomeUser' }" size="medium" flat>Beranda</q-btn>
              <q-btn :to="{ name: 'myOrder' }" size="medium" flat>Pesanan Saya</q-btn>
              <q-btn  size="medium" flat> About </q-btn>
              <q-btn  size="medium" @click="logout" flat> Logout </q-btn>
        </div>
             <q-btn  flat @click="drawer = ! drawer" round v-if="$q.screen.xs" icon="menu"/>
    </q-toolbar>
         <q-drawer
          v-model="drawer"
          :width="200"
          :breakpoint="500"
          side="left"
        >
          <q-scroll-area class="fit">
          <q-list padding class="menu-list">

              <q-item clickable  v-ripple :to="{ name: 'HomeUser' }">
                <q-item-section avatar>
                  <q-icon name="home" />
                </q-item-section>
                <q-item-section>
                 Beranda
                </q-item-section>
              </q-item>
              <q-item clickable  v-ripple :to="{ name: 'myOrder' }">
                <q-item-section avatar>
                  <q-icon name="inventory" />
                </q-item-section>
                <q-item-section>
                 Pesanan Saya
                </q-item-section>
              </q-item>
              <q-item clickable  v-ripple >
                 <q-item-section avatar >
                   <q-icon name="announcement"/>
                </q-item-section>
                <q-item-section>
                 About
                </q-item-section>
              </q-item>
              <q-item clickable  v-ripple  @click="logout">
                <q-item-section avatar>
                  <q-icon name="logout" />
                </q-item-section>
                <q-item-section>
                 Logout
                </q-item-section>
              </q-item>
              <q-separator />

          </q-list>
        </q-scroll-area>
      </q-drawer>
   <q-page-container class="bg-blue-grey-3">
      <router-view />
    </q-page-container>
 </q-layout>
</template>
<script>
export default {
  data () {
    return {
      drawer: false
    }
  },
  methods: {
    logout () {
      this.$q.localStorage.remove()
      this.$router.push({ name: 'loginPage' })
    }
  }
}
</script>
