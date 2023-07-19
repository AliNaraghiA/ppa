<template>
  <div @mouseleave="mouseOut">
    <div
      class="circles"
      :style="{ height: divVh, width: divVw }"
      @mouseenter="mouseEnter"
      @mousemove="mouseMove"
    ></div>
  </div>
</template>

<script>
export default {
  props: ["myVw", "myVh"],
  data() {
    return {
      mouse: {
        x: 0,
        y: 0,
      },
      requestId: null,
      lines: [],
      vw: 0,
      vh: 0,
      divVw: "",
      divVh: "",
      width: 3,
      height: 3,
    };
  },
  mounted() {
    setTimeout(() => {
      this.vw = this.myVw;
      this.vh = this.myVh;
      this.divVw = this.myVw + "px";
      this.divVh = this.myVh + "px";
      for (var y = 10; y <= this.vh - this.height; y += 40) {
        for (var x = 10; x <= this.vw - this.width; x += 40) {
          var line = document.createElement("div");
          document.getElementsByClassName("circles")[0].append(line);
          line.style.width = this.width + "px";
          line.style.height = this.height + "px";
          line.style.left = x + "px";
          line.style.top = y + "px";
          line.className = "circle2";
          this.lines.push({
            element: line,
            cx: x + this.width / 2,
            cy: y + this.height / 2,
          });
        }
      }
      this.changeColor;
    }, 10);
  },
  methods: {
    mouseEnter() {
      this.$store.commit("mouseEffectOff");
      const circles = document.getElementsByClassName("circle2");
      for (let i = 0; i < circles.length; i++) {
        circles[i].style.width = "13px";
        circles[i].style.backgroundColor = "white";
        circles[i].style.borderRadius = "0%";
        circles[i].style.transform = "translateX(-5px)";
      }
    },
    mouseMove(event) {
      this.mouse.x = event.layerX;
      this.mouse.y = event.layerY;

      if (!this.requestId) {
        this.requestId = requestAnimationFrame(this.update);
      }
    },
    update() {
      for (var i = 0; i < this.lines.length; i++) {
        var line = this.lines[i];

        var dx = this.mouse.x - line.cx;
        var dy = this.mouse.y - line.cy;

        var transform = "rotate(" + Math.atan2(dy, dx) + "rad)";
        line.element.style.transform = transform;
        if (dx < -300 || dx > 300 || dy < -300 || dy > 300) {
          line.element.style.transition = "width 0.5s";
          line.element.style.width = "5px";
        } else if (dx < -250 || dx > 250 || dy < -250 || dy > 250) {
          line.element.style.transition = "width 0.5s";
          line.element.style.width = "7px";
        } else if (dx < -200 || dx > 200 || dy < -200 || dy > 200) {
          line.element.style.transition = "width 0.5s";
          line.element.style.width = "9px";
        } else if (dx < -150 || dx > 150 || dy < -150 || dy > 150) {
          line.element.style.transition = "width 0.5s";
          line.element.style.width = "11px";
        } else {
          line.element.style.transition = "width 0.5s";
          line.element.style.width = "13px";
        }
      }

      this.requestId = null;
    },
    mouseOut() {
      this.$store.commit("mouseEffectOn");
      const circles = document.getElementsByClassName("circle2");
      for (let i = 0; i < circles.length; i++) {
        circles[i].style.width = "3px";
        circles[i].style.backgroundColor = "";
        circles[i].style.borderRadius = "50%";
        circles[i].style.transform = "translateX(5px)";
      }
      this.changeColor;
    },
  },
  computed: {
    changeColor() {
      const circle = document.getElementsByClassName("circle2");
      for (let i = 0; i < circle.length; i++) {
        const random = Math.floor(Math.random() * 11);
        if (random > 5) {
          circle[i].className = "circle2 grayCircle2";
        } else {
          circle[i].className = "circle2 whitCircle2";
        }
      }
      setInterval(() => {
        for (let i = 0; i < circle.length; i++) {
          if (circle[i].className == "circle2 whitCircle2") {
            circle[i].className = "circle2 grayCircle2";
          } else {
            circle[i].className = "circle2 whitCircle2";
          }
        }
      }, 2000);
    },
  },
};
</script>

<style>
</style>