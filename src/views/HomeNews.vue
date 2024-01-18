<template>
  <div>
    <FeedContent :visible-articles="visibleArticles" :articles-data="articlesData" @update-articles="updateArticles" />
  </div>
</template>

<script>
import FeedContent from '../components/FeedContent.vue';

export default {
  name: 'HomeNews',
  components: {
    FeedContent
  },
  props: {
    visibleArticles: {
      type: Number,
      default: 10,
    }
  },
  data() {
    return {
      articlesData: [],
      pageSize: 10
    };
  },
 methods: {
    async fetchArticles(query , pageSize=10) {
      try {
      const apiKey = process.env.VUE_APP_NEWS_KEY;
      const date = new Date(new Date().valueOf() - 1000 * 60 * 60 * 24).toISOString().slice(0, 10).replaceAll('-', '/');
      const url = `https://newsapi.org/v2/everything?apiKey=${apiKey}&from=${date}&pageSize=${pageSize}&q=${query}`;
      const response = await fetch(url, {
        method: 'GET',
        headers: {
          'x-api-key': apiKey,
        },
      });

      const data = await response.json();

      if (data.status === 'error') {
        console.error('data.status shows:',data.message);
        // Handle the error gracefully (e.g., show an error message to the user)
        return;
      }

      // Update articlesData with the retrieved articles
      this.articlesData = data.articles.slice(0, pageSize);
    } catch (error) {
      console.error('Error fetching articles:', error);
      // Handle the error gracefully (e.g., show an error message to the user)
    }
  
    },
      addArticles() {
          const searchQuery = document.querySelector('.search').value;
          this.pageSize = this.pageSize + 10;
          this.fetchArticles(searchQuery || this.query, this.pageSize);
      },
      openArticleDetail(article) {
    // Instead of setting showArticleDetail to true, navigate to the ArticleDetail route
        this.$router.push({ name: 'ArticleDetail', params: { id: article.id } });
      },
    }

}
</script>

<style scoped>
/* Add specific styles for Home.vue if needed */
</style>
