<template>
  <div class="flex row justify-center truncate h-48">
    <div class="flex row justify-center w-42 rotate-12 p-5 pt-12 " :class="className">
      <div
        class="child left border-dotted  border-indigo-600 w-16 h-24 "
        :data-id="item.id"
        data-side="left"
      ></div>
      <div
        class="child right border-dotted  border-indigo-600 w-16 h-24  "
        :data-id="item.id"
        data-side="right"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },

  props: {
    item: {},
  },
  computed:{
    className(){
      return this.item.left && this.item.right?'bug2':'bug1'
    }
  },
  methods: {
    log(event) {
      console.log(event);
    },
  },
};
</script>

<style scoped>
.bug1{
  background: url('/img/bug1.png');
  background-repeat: no-repeat;
}
.bug2{
  background: url('/img/bug2.png');
  background-repeat: no-repeat;
}
</style>
