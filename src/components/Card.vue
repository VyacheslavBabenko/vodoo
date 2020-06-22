<template>
  <div class="card">
    <div class="card__title">{{ post.title }}</div>
    <div class="card__description">
      {{ post.body }}
    </div>
    <div class="card__author">{{ post.author }}</div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: ["post"],
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import "node_modules/bootstrap/scss/bootstrap";
@import "node_modules/bootstrap-vue/src/index.scss";

.card {
  padding: 30px;
  text-align: left;

  &__title {
    margin-bottom: 10px;
    color: blue;
  }

  &__description {
    margin-bottom: 10px;
  }

  &__author {
    color: gray;
  }
}
</style>
