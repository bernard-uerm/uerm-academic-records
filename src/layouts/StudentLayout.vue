<template>
  <q-layout view="lHh Lpr lFf" class="bg-image">
    <q-header elevated v-if="studentIsLoggedIn" style="background:linear-gradient(to right, #26A69A , #1976D2)">
      <q-toolbar>
        <q-toolbar-title class="text-right text-uppercase">
          UERM Student Portal
        </q-toolbar-title>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
          v-if="isLoggedIn"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class=""
      v-if="studentIsLoggedIn"
      side="right"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          <q-card>
            <q-card-section align="center">
              {{ studentCredentials.NAME }}
            </q-card-section>
            <q-card-section align="center">
              <q-btn color="primary" @click="logout" icon="logout" label="log out"></q-btn>
            </q-card-section>
          </q-card>
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'

const linksList = [
  {
    title: 'Dashboard',
    caption: 'Your Dashboard',
    icon: 'fa fa-chart-area',
    link: 'https://quasar.dev'
  },
  {
    title: 'Clearance',
    caption: 'Student Clearance',
    icon: 'school',
    link: 'https://quasar.dev'
  },
];

import { mapGetters } from 'vuex'
export default {
  name: 'MainLayout',
  components: {
    EssentialLink
  },
  data () {
    return {
      isLoggedIn: false,
      leftDrawerOpen: false,
      essentialLinks: linksList,
      loading: null
    }
  },
  computed: {
    ...mapGetters({
      studentCredentials: 'students/studentCredentials',
      studentIsLoggedIn: 'students/isLoggedIn'
    })
  },
  mounted () {
    this.checkAuthentication()
  },
  methods: {
    async checkAuthentication () {
      this.loading = true
      if (this.$q.localStorage.has('studentLogin')) {
        let studentID = this.$q.localStorage.getItem('studentID')
        const studentInfo = {
          studentNo: studentID,
          checking: true
        }
        const validatedStudent = await this.$store.dispatch('students/validateStudent', studentInfo)
        console.log(this.studentIsLoggedIn)
        this.isLoggedIn = this.studentIsLoggedIn
      }
      this.loading = false
    },
    async logout () {
      await this.$store.dispatch('students/logout')
      this.$router.push('/')
    }
  }
}
</script>


<style>
.bg-image {
  background-image: url("~assets/bg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
</style>