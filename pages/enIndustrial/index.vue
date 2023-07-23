<template>
    <div class="industrial">
        <div class="enIndustrial mont">
            <Header lang="eng" @change="change" />
            <div class="topDiv">
                <h3 class="mont">Products</h3>
                <h1>Industrial products </h1>
                <p>
                    Aluminum with its silvery color and low specific weight has a very high application in industries. Based on the different properties of products and their uses, aluminum profiles can be produced in various dimensions and shapes. 
                </p>
            </div>

            <div class="lineDiv">
                <Lines myVw="1920" myVh="100" />
            </div>
            <div class="myUnits">
                <section v-for="item in industrial" :key="item.id">
                    <div
                        v-if="item.id % 2 != 0"
                        class="item1 d-flex justify-content-between align-items-center">
                        <div>
                            <div class="titleAndText">
                                <h5 class="title">{{ item.title }}</h5>
                                <p class="text">{{ item.text }}</p>
                            </div>
                            <router-link
                                :to="`/enIndustrial/${item.id}`"
                                class="effectBtn">
                                <span>info</span>
                            </router-link>
                        </div>
                        <img :src="item.image" alt="" />
                    </div>
                    <div
                        v-else
                        class="item2 d-flex justify-content-between align-items-center">
                        <img :src="item.image" alt="" />
                        <div>
                            <div class="titleAndText">
                                <h5 class="title">{{ item.title }}</h5>
                                <p class="text">{{ item.text }}</p>
                            </div>
                            <router-link
                                :to="`/enIndustrial/${item.id}`"
                                class="effectBtn">
                                <span>info</span>
                            </router-link>
                        </div>
                    </div>
                </section>
            </div>

            <div class="footer">
                <Footer lang="en" />
            </div>
        </div>
        <MouseEffect />
    </div>
</template>

<script>
import gsap from "gsap";
import ScrollTrigger from "gsap/dist/ScrollTrigger";
export default {
    data() {
        return {
            language: "",
            industrial: [
                {
                    title: "bar",
                    text: "Aluminum rod (aluminum rebar) is a low-density silver metal rod. This metal has many applications in various industries. The main element of this aluminum rod is aluminum, but different alloys can be produced from it based on the required properties.",
                    id: "bar",
                    image: "/images/industrial1.png",
                },
                {
                    title: "belt",
                    text: "A silver aluminum strip that is used in the construction of handles and various fittings. This aluminum belt is formed under pressure and is available in both plain and three-line variations in the market.",
                    id: "belt",
                    image: "/images/industrial2.png",
                },
                {
                    title: "pipe",
                    text: "Nowadays, aluminum pipes and aluminum sheets are one of the most widely used equipment in both large and small, light and heavy industries. The thickness and size of aluminum pipes are determined based on their application.",
                    id: "pipe",
                    image: "/images/industrial3.png",
                },
                {
                    title: "hexagonal-kelly",
                    text: "Hexagonal aluminum refers to the hexagonal shape that is produced in a T-shaped and extruded form using the desired alloys. They are available in various sizes and can be supplied to you based on your order and needs.",
                    id: "hexagonal-kelly",
                    image: "/images/industrial3.png",
                },
            ],
        };
    },
    mounted() {
        this.language = this.$store.state.lang;
        this.unitAnimation();
    },
    methods: {
        change() {
            this.$store.commit("change");
        },
        unitAnimation() {
            gsap.registerPlugin(ScrollTrigger);

            gsap.set(".myUnits", { autoAlpha: 1 });

            var allSections = gsap.utils.toArray("section");
            var allSectionsNotFirst = allSections.slice(1);
            gsap.set(allSectionsNotFirst, { position: "absolute" });

            var dur = 0.5,
                next = 20,
                dur = 14,
                tt = 3000;
            var action = gsap
                .timeline({
                    defaults: {
                        duration: dur,
                        ease: "none",
                        stagger: next,
                    },
                })
                .to(allSectionsNotFirst, { yPercent: -100 })
                .to({}, { duration: 1 });

            ScrollTrigger.create({
                trigger: ".myUnits",
                start: "top top",
                end: "+=" + tt,
                pin: true,
                animation: action,
                scrub: 0.3,
            });
        },
    },
    watch: {
        "$store.state.lang"() {
            this.language = this.$store.state.lang;
        },
    },
};
</script>

<style></style>
