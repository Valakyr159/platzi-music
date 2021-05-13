<template lang="pug">
  section.hero.is-success
    .hero-head
      header.nav
        .container
          .nav-left
            .nav-item
              strong 📻 Platzi Music  
          .nav-right.nav-menu

    .hero-body
      .container.has-text-centered
        h1.title Platzi Music
        h2.subtitle Canciones que están Vue-nísimas
        PmPlayer

</template>

<script>
import PmPlayer from '@/components/Player'
export default {
  components: { PmPlayer },
}
</script>

<style lang="scss" scoped>
.hero-head {
  display: flex;
  justify-self: flex-start;
}
</style>
