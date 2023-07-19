<template>
  <div class="balls" v-show="$store.state.mouseEffect">
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
    <div class="ball"></div>
  </div>
</template>

<script>
import gsap from "gsap";
export default {
  data() {
    return {
      color1: "(#ffffff 30%, #1BFFFF 70%)",
      color2: "(#ffffff 30%, #FBB03B 70%)",
      color3: "(#ffffff 30%, #FCEE21 70%)",
      color4: "(#ffffff 30%, #ED1E79 70%)",
      color5: "(#ffffff 30%, #A890FE 70%)",
      empty: "",
      targets: null,
      moveTimer: null,
    };
  },
  mounted() {
    const balls = document.getElementsByClassName("ball");
    gsap.set(".ball", { xPercent: -50, yPercent: -50 });
    this.targets = gsap.utils.toArray(".ball");

    for (let j = 0; j < balls.length; j++) {
      balls[j].style.background = `radial-gradient${this.color1}`;
    }
    setInterval(() => {
      this.empty = this.color1;
      this.color1 = this.color2;
      this.color2 = this.color3;
      this.color3 = this.color4;
      this.color4 = this.color5;
      this.color5 = this.empty;
      for (let j = 0; j < balls.length; j++) {
        balls[j].style.background = `radial-gradient${this.color1}`;
      }
    }, 500);
    window.addEventListener("mousemove", (e) => {
      clearTimeout(this.moveTimer);
      this.moveTimer = setTimeout(function () {
        for (let i = 0; i < balls.length; i++) {
          balls[i].style.display = "none";
        }
      }, 500);
      for (let i = 0; i < balls.length; i++) {
        balls[i].style.display = "block";
      }
      gsap.to(this.targets, {
        duration: 0.1,
        x: e.pageX,
        y: e.pageY,
        ease: "none",
        overwrite: "auto",
        stagger: 0.035,
      });
    });
  },
};
</script>

<style>
</style>