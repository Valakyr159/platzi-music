<template lang="pug">
  .container
    .columns
      .column.is-2.is-offset-4
        .notification(:class="notificationColor ? 'is-danger' : 'is-primary'")
          slot(name="body") Algo salió mal
</template>

<script>
export default {
  props: {
    notificationColor: {
      type: Boolean,
      required: true,
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
}
.notification {
  margin-top: -32px;
  margin-left: -160px;
  position: absolute;
  z-index: 100;
  width: 300px;
}
</style>
