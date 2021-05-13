<template>
  <div id="app">
    <PmHeader />
    <PmNotification v-show="showNotification" :notificationColor="notFound">
      <p v-show="notFound" slot="body">No se encontraron resultados</p>
      <p v-show="!notFound" slot="body">
        {{ searchMessage }}
      </p>
    </PmNotification>
    <PmLoader v-show="isLoading" />
    <section v-show="!isLoading" class="section">
      <nav class="navbar">
        <div class="container">
          <input
            class="input is-large"
            type="text"
            placeholder="Buscar canciones"
            v-model="searchQuery"
          />
          <a @click="search" class="button is-info is-large">Buscar</a>
          <a class="button is-danger is-large">X</a>
        </div>
      </nav>
      <div class="container">
        <p v-show="this.tracks.length && !showNotification">
          <small>{{ searchMessage }}</small>
        </p>
      </div>
      <div class="container results">
        <div class="columns is-multiline">
          <div class="column is-one-quarter" v-for="t in tracks" :key="t.id">
            <PmTrack
              :class="{ 'is-active': t.id == selectedTrack }"
              :track="t"
              @select="setSelectedTrack"
            />
          </div>
        </div>
      </div>
    </section>
    <PmFooter class="footer" />
  </div>
</template>

<script>
import trackService from '@/services/track'

import PmFooter from '@/components/layout/Footer'
import PmHeader from '@/components/layout/Header'

import PmTrack from '@/components/Track'

import PmNotification from '@/components/shared/Notification'
import PmLoader from '@/components/shared/Loader'

export default {
  name: 'App',
  data() {
    return {
      searchQuery: '',
      tracks: [],
      isLoading: false,
      notFound: false,
      selectedTrack: '',
      showNotification: false,
    }
  },
  components: { PmFooter, PmHeader, PmTrack, PmLoader, PmNotification },
  methods: {
    setSelectedTrack(id) {
      this.selectedTrack = id
    },
    search() {
      if (!this.searchQuery) {
        return
      }

      this.isLoading = true

      trackService.search(this.searchQuery).then((res) => {
        this.notFound = res.tracks.total === 0
        this.tracks = res.tracks.items
        this.isLoading = false
        this.showNotification = true
      })
    },
  },
  computed: {
    searchMessage() {
      return `Encontrados: ${this.tracks.length}`
    },
  },
  watch: {
    showNotification() {
      if (this.showNotification) {
        setTimeout(() => {
          this.showNotification = false
        }, 3000)
      }
    },
  },
}
</script>

<style lang="scss">
@import '@/scss/main.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: auto;
}

.results {
  margin-top: 50px;
}

.footer {
  position: relative;
  bottom: 0;
  left: 0;
  right: 0;
}

.is-active {
  border: 3px #23d160 solid;
}
</style>
