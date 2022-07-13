<template>
  <div class=" border-2 border-solid border-color-red-700 w-16 h-24 bg-red-600" 
    :class="side=='left'?'left rounded-l-3xl':'right rounded-r-3xl'"
    :data-dots="dots">
    <img :src="'/img/'+dots+'.png'" alt="">
  </div>  
</template>

<script>
import { Container, Draggable } from "vue-dndrop";
export default {
  components: { Container, Draggable },
props:{
  side:{},
  dots:{}
},
data() {
    return {
    };
  },

  methods: {

  }  
};
</script>

<style scoped>
  .left img{

      transform: scaleX(-1);

  }
</style>
