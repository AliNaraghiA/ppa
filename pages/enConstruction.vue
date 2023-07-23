<template>
    <div class="construction">
        <div class="enConstruction mont">
            <Header lang="eng" @change="change" />
            <div class="topDiv">
                <h3 class="mont">Products</h3>
                <h1>Ingots & billets</h1>
                <p>
                    Today, one of the most diverse and commonly used aluminum parts in the construction industry is ingots and billets. Due to its resistance to corrosion and oxidation, aluminum is widely used for producing ingots and billets. 

                </p>
            </div>
            <div class="lineDiv">
                <Lines myVw="1920" myVh="100" />
            </div>
            <div class="categories d-flex justify-content-between">
                <div @click="category = 1" :class="{ active: category == 1 }">
                    Normal drawer system (Series 60)                </div>
                <div @click="category = 2" :class="{ active: category == 2 }">
                    Normal roller system (Series 47)
                </div>
                <div @click="category = 3" :class="{ active: category == 3 }">
                    Mesh grille system
                </div>
                <div @click="category = 4" :class="{ active: category == 4 }">
                    Other
                </div>
            </div>
            <div
                v-if="filteredConstructions.length"
                class="items d-flex justify-content-between flex-wrap">
                <div
                    v-for="construction in filteredConstructions"
                    :key="construction.databaseId">
                    <div
                        v-for="(item, index) in construction.acfconstructions
                            .repeater"
                        :key="index"
                        class="item">
                        <div class="whiteDiv">
                            <img
                                :src="item.image.sourceUrl"
                                :alt="item.image.altText" />
                        </div>
                        <h5 class="title">{{ item.title }}</h5>
                        <div class="name">{{ item.name }}</div>
                        <div class="weight mont">{{ item.weight }}</div>
                    </div>
                </div>
            </div>
            <div class="footer">
                <Footer lang="en" />
            </div>
        </div>
        <MouseEffect />
    </div>
</template>

<script>
import gql from "graphql-tag";
export default {
    data() {
        return {
            language: "",
            category: 1,
            englishconstructions: [],
        };
    },
    async asyncData({ app }) {
        const query = gql`
query Constructions {
    englishconstructions {
    nodes {
      desiredSlug
      databaseId
      slug
      acfconstructions {
        category
        repeater {
          image {
            altText
            sourceUrl
          }
          name
          weight
          title
        }
      }
    }
  }
}
        `;

        const { data } = await app.apolloProvider.defaultClient.query({
            query,
        });

        return {
            englishconstructions: data.englishconstructions.nodes,
        };
    },
    computed: {
        filteredConstructions() {
            return this.englishconstructions.filter(
                (englishconstructions) =>
                englishconstructions.acfconstructions.category == this.category
            );
        },
    },
    //lanG
    mounted() {
        this.language = this.$store.state.lang;
    },
    methods: {
        change() {
            this.$store.commit("change");
        },
    },
    watch: {
        "$store.state.lang"() {
            this.language = this.$store.state.lang;
        },
    },
};
</script>
