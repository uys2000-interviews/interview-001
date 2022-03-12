<template>
  <div ref="touch" class="scene">
    <div class="pages" :style="moveImageLeft">
      <div class="page" :style="imageLeft + rotateImageLeft">asd</div>
    </div>
    <div class="pages" :style="moveImage">
      <div class="page" :style="imageCenter + rotateImage">asd</div>
    </div>
    <div class="pages" :style="moveImageRight">
      <div class="page" :style="imageRight + rotateImageRight">asd</div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  inject: ["page"],
  data() {
    return {
      onMove: false,
      direct: 1,
      startX: 0,

      lastL: 0,
      last: 0,
      lastR: 0,

      indexL: 0,
      index: 1,
      indexR: 0,
      positions: {},

      mR: 3.2,
      mZ: 10,

      left: { X: 0, Z: 0, R: 0 },
      center: { X: 0, Z: 0, R: 0 },
      right: { X: 0, Z: 0, R: 0 },

      p: 0,
    };
  },
  methods: {
    setDefault: function () {
      this.positions["-2"] = -window.innerWidth;
      this.positions["-1"] = -window.innerWidth / 2;
      this.positions["0"] = 0;
      this.positions["1"] = window.innerWidth / 2;
      this.positions["2"] = window.innerWidth;
      this.left = this.setPageLR();
      this.lastL = this.left.X;
      this.right = this.setPageLR(false);
      this.lastR = this.right.X;
    },

    setPageLR: function (L = true) {
      var x;
      if (L) x = this.positions["1"];
      else x = this.positions["-1"];
      return { X: x, R: x / this.mR, Z: this.setInnerZ(x * this.mZ), L: x };
    },
    setPageCenter: function () {
      const x = 0;
      return x, 0, 0;
    },
    getDirection: function (x) {
      if (x < 0) return -1;
      else return 1;
    },
    pageSlideCheck: function (x, direct) {
      x = x * direct;
      if (x > window.innerWidth / 5) {
        if (this.p == 0) this.page = this.pageChangeCheck();
        this.setPageIndex();
        return true;
      } else {
        if (this.p == 1) this.page = this.pageChangeCheck();
        this.setPageIndex();
        return false;
      }
    },
    pageChangeCheck: function () {
      this.p = 1;
      const cache = this.page + 1 * this.direct;
      if (cache < -1 || 1 < cache) return this.page;
      else return cache;
    },
    setInnerZ: function (x) {
      if (x > 0) return x * -1;
      else return x;
    },
    pageSlideEvent: function (x) {
      return {
        X: x,
        R: x / this.mR,
        Z: this.setInnerZ(x * this.mZ),
      };
    },
    setPageIndex: function (ret) {
      if (this.page == -1) {
        this.indexL = 0;
        this.index = 1;
        this.indexR = 2;
      } else if (this.page == 0) {
        this.indexL = 0;
        this.index = 1;
        this.indexR = 0;
      } else {
        this.indexL = 2;
        this.index = 1;
        this.indexR = 0;
      }
      return ret;
    },
    pageSlideStarter: function (x) {
      this.startX = x;
      this.onMove = true;
    },
    pageSlider: function (x) {
      const X = (x - this.startX) / 2;
      this.direct = this.getDirection(X);
      this.sCheck = this.pageSlideCheck(X, this.direct);

      this.left = this.pageSlideEvent(X - this.lastL);
      this.center = this.pageSlideEvent(X - this.last);
      this.right = this.pageSlideEvent(X - this.lastR);
    },
    pageSlideEnder: function () {
      this.onMove = false;

      if (this.sCheck);
      this.left = this.pageSlideEvent(this.positions[`${this.page - 1}`]);
      this.center = this.pageSlideEvent(this.positions[`${this.page}`]);
      this.right = this.pageSlideEvent(this.positions[`${this.page + 1}`]);

      this.lastL = -this.left.X;
      this.last = -this.center.X;
      this.lastR = -this.right.X;
      this.p = 0;
      console.log(`
Direction : ${this.direct}
sCheck : ${this.sCheck}
left : ${this.left.X}
leftL : ${this.lastL}
center : ${this.center.X}
centerL : ${this.last}
right : ${this.right.X}
rightL : ${this.lastL}
page: ${this.page}
`);
    },
  },
  created() {
    this.setDefault();
  },
  mounted() {
    this.$refs["touch"].addEventListener("touchstart", (event) => {
      this.pageSlideStarter(event.touches[0].clientX);
    });
    this.$refs["touch"].addEventListener("touchmove", (event) => {
      this.pageSlider(event.touches[0].clientX);
    });
    this.$refs["touch"].addEventListener("touchend", () => {
      this.pageSlideEnder();
    });

    this.$refs["touch"].addEventListener("mousedown", (event) => {
      this.pageSlideStarter(event.clientX);
    });
    this.$refs["touch"].addEventListener("mousemove", (event) => {
      if (this.onMove) this.pageSlider(event.clientX);
    });
    this.$refs["touch"].addEventListener("mouseup", () => {
      this.pageSlideEnder();
    });
  },
  computed: {
    moveImageLeft: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translate3d(${this.left.X}px, 0px, 0px); z-index: ${this.indexL}; ${style}`;
    },
    moveImage: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translate3d(${this.center.X}px, 0px, 0px); z-index: ${this.index}; ${style}`;
    },
    moveImageRight: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translate3d(${this.right.X}px, 0px, 0px); z-index: ${this.indexR}; ${style}`;
    },
    rotateImageLeft: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translateZ(${this.left.Z}px)
      rotateY(${this.left.R}deg); ${style}`;
    },
    rotateImage: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translateZ(${this.center.Z}px)
      rotateY(${this.center.R}deg); ${style}`;
    },
    rotateImageRight: function () {
      var style;
      if (!this.onMove) style = "transition-duration: 1.5s;";
      return `transform: translateZ(${this.right.Z}px)
      rotateY(${this.right.R}deg); ${style}`;
    },
    imageLeft: function () {
      return `background: url("${require("@/assets/left.jpg")}"); 
      background-position: center;
      background-repeat: no-repeat; 
      background-size: cover;`;
    },
    imageCenter: function () {
      return `background: url("${require("@/assets/center.jpg")}"); 
      background-position: center;
      background-repeat: no-repeat; 
      background-size: cover;`;
    },
    imageRight: function () {
      return `background: url("${require("@/assets/right.jpg")}"); 
      background-position: center;
      background-repeat: no-repeat; 
      background-size: cover;`;
    },
  },
};
</script>
