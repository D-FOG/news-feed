<template>
  <div>
    <NavBar :query="query" @search="fetchArticles" />
    <div class="article-detail">
      <h1>{{ selectedArticle.title }}</h1>
      <div :style="{ backgroundImage: `url(${selectedArticle.image})` }" class="article-image"></div>
      <p>{{ selectedArticle.fullArticle }}</p>
      <p class="source">Source: {{ selectedArticle.source }}</p>
    </div>
  </div>
</template>

<script>
import NavBar from '../components/NavBar.vue';

export default {
  components: {
    NavBar,
  },
  props: {
    selectedArticle: {
      type: Object,
      default: () => ({}),
    },
  },
  computed: {
    // Instead of using the selectedArticle prop directly, fetch details based on the route parameter
    articleId() {
      return this.$route.params.id;
    },
    // Now use articleId to fetch the specific article details
    articleDetails() {
      // Perform an API request to fetch details based on articleId
      // You may need to modify this based on your API structure
      // This is just a placeholder example
      return { title: 'Article Title', content: 'Full article content', source: 'Article Source' };
    },
  },
};
</script>

<style lang="scss">
// Use existing styles or add new styles as needed
</style>
