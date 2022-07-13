<template>
  <div class="w-52">
    <img class="absolute" src="/img/mom.png" alt="">
    <img class="absolute" :src="'/img/mom_'+dots+'.png'" alt="">
  </div>
</template>

<script>

export default {

  data() {
    return {};
  },

  props: {
    dots: {},
  },
  computed:{

  },
  methods: {

  },
};
</script>

<style scoped>
</style>
