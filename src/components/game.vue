<template>
  <div class="h-screen flex flex-col ">
  <div class="row flex gap-5 justify-between bg-green-700">
    <div class="grid grid-cols-3 p-5 gap-6">
      <div class="wing rotate-12" v-for="item in leftWings">
        <Wing :dots="item.dots" side="left"></Wing>
      </div>
    </div>
    <div>
      <Mom :dots="mother" />
    </div>
    <div class="grid grid-cols-3 p-5 gap-6 ">
      <div class="wing rotate-12" v-for="item in rightWings">
        <Wing :dots="item.dots" side="right"></Wing>
      </div>
    </div>
  </div>
  <div class="grid grid-cols-3 grow bg-green-400 pt-5">
    <Child v-for="child in children" :item="child" :dataId="child.id"></Child>
  </div>
  </div>
</template>

<script>
import Child from "./child.vue";
import Mom from "./mom.vue";
import Wing from "./wing.vue";
export default {
  name: "Groups",

  components: { Child, Wing, Mom },

  data() {
    return {
      moving: null,
      drake: {},
      leftWings: [

      ],
      rightWings: [

      ],
      mother: 5,
      children: [
        { id: 0, left: null, right: null },
        { id: 1, left: null, right: null },
        { id: 2, left: null, right: null },
        { id: 3, left: null, right: null },
        { id: 4, left: null, right: null },
        { id: 5, left: null, right: null },
      ],
    };
  },
  mounted() {
    
    this.mother = Math.floor(Math.random() * 8)+2;
    for (let i = 0; i < 6; i++) {
      var currentDots = Math.floor(Math.random() * (this.mother+1));

      this.leftWings.push({id:i,dots:currentDots})
      this.rightWings.push({id:i,dots:this.mother - currentDots})
    }

    var $this = this;
    this.drake = dragula({
      isContainer: function (el) {
        return el.classList.contains("wing") || el.classList.contains("child");
      },
      accepts: function (el, target) {
        if (target.classList.contains("wing")) return false;
        if (
          (target.classList.contains("left") &&
            el.classList.contains("left")) ||
          (target.classList.contains("right") && el.classList.contains("right"))
        ) {
          var id = target.getAttribute("data-id");
          var side = target.getAttribute("data-side");
          var dots = el.getAttribute("data-dots");
          var otherSide = side == "left" ? "right" : "left";
          var it = $this.children.find((i) => i.id == id);

          if (it[side] == null) {
            if(it[otherSide]==null) return true;
            var sum = parseInt(it[otherSide])+parseInt(dots)
            if(sum==$this.mother) return true;
          }
        }
        return false;
      },

    });

    this.drake.on("drop", (el, target, source, sibling) => {
      var id = target.getAttribute("data-id");
      var side = target.getAttribute("data-side");
      var dots = el.getAttribute("data-dots");
      var it = $this.children.find((i) => i.id == id);
      
      var sourceId = source.getAttribute("data-id");
      var sourceSide = source.getAttribute("data-side");
      if(sourceId && sourceSide){
        var sourceIt = $this.children.find((i) => i.id == sourceId);
        sourceIt[sourceSide]=null
      }
      it[side] = dots;
    });
    
  },
  methods: {
    pickup(event) {
      this.moving = event.target;
      var moving = this.moving;

      moving.style.height = moving.clientHeight + 2 + "px";
      moving.style.width = moving.clientWidth + 2 + "px";
      moving.style.position = "fixed";
      moving.style.zIndex = "-10";
    },
    move2(event) {
      console.log(event);
    },
    move(event) {
      var moving = this.moving;
      if (moving) {
        if (event.clientX) {
          // mousemove
          moving.style.left = event.clientX - moving.clientWidth / 2 + "px";
          moving.style.top = event.clientY - moving.clientHeight / 2 + "px";
        } else {
          // touchmove - assuming a single touchpoint
          moving.style.left =
            event.changedTouches[0].clientX - moving.clientWidth / 2 + "px";
          moving.style.top =
            event.changedTouches[0].clientY - moving.clientHeight / 2 + "px";
        }
      }
    },
    drop(event) {
      var moving = this.moving;
      if (moving) {
        if (event.currentTarget.tagName !== "HTML") {
          let target = null;
          if (event.clientX) {
            target = document.elementFromPoint(event.clientX, event.clientY);
          } else {
            target = document.elementFromPoint(
              event.changedTouches[0].clientX,
              event.changedTouches[0].clientY
            );
          }

          target.appendChild(moving);
        }
        // reset our element
        moving.style.left = "";
        moving.style.top = "";
        moving.style.height = "";
        moving.style.width = "";
        moving.style.position = "";

        moving = null;
      }
    },
  },
};
</script>

<style>
.gu-mirror {
  position: fixed !important;
  margin: 0 !important;
  z-index: 9999 !important;
  opacity: 0.8;
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=80)";
  filter: alpha(opacity=80);
}
.gu-hide {
  display: none !important;
}
.gu-unselectable {
  -webkit-user-select: none !important;
  -moz-user-select: none !important;
  -ms-user-select: none !important;
  user-select: none !important;
}
.gu-transit {
  opacity: 0.2;
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=20)";
  filter: alpha(opacity=20);
}
</style>
