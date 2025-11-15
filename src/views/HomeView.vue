<template>
  <div class="home">
    <div class="hero">
      <h1>📰 CNN NEWS</h1>
      <p>Facts First</p>
    </div>

    <SearchBar @search="handleSearch" />
    <CategoryFilter :selectedCategory="selectedCategory" @select="handleCategorySelect" />

    <div v-if="loading" class="loading">
      <p>Memuat berita...</p>
    </div>

    <div v-else-if="filteredNews.length === 0" class="no-results">
      <p>❌ Tidak ada berita ditemukan</p>
    </div>

    <div v-else class="news-grid">
      <NewsCard v-for="news in filteredNews" :key="news.id" :news="news" />
    </div>
  </div>
</template>

<script>
import NewsCard from '../components/NewsCard.vue';
import CategoryFilter from '../components/CategoryFilter.vue';
import SearchBar from '../components/SearchBar.vue';

export default {
  name: 'HomeView',
  components: {
    NewsCard,
    CategoryFilter,
    SearchBar
  },
  data() {
    return {
      allNews: [],
      filteredNews: [],
      selectedCategory: 'Semua',
      searchQuery: '',
      loading: true
    }
  },
  async mounted() {
    await this.loadNews();
  },
  methods: {
    async loadNews() {
      try {
        const response = await fetch('/news-data.json');
        const data = await response.json();
        this.allNews = data.news;
        this.filteredNews = this.allNews;
        this.loading = false;
      } catch (error) {
        console.error('Error loading news:', error);
        this.loading = false;
      }
    },
    handleCategorySelect(category) {
      this.selectedCategory = category;
      this.filterNews();
    },
    handleSearch(query) {
      this.searchQuery = query;
      this.filterNews();
    },
    filterNews() {
      let filtered = this.allNews;

      // Filter berdasarkan kategori
      if (this.selectedCategory !== 'Semua') {
        filtered = filtered.filter(news => news.category === this.selectedCategory);
      }

      // Filter berdasarkan pencarian
      if (this.searchQuery.trim() !== '') {
        filtered = filtered.filter(news => 
          news.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          news.summary.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }

      this.filteredNews = filtered;
    }
  }
}
</script>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.hero {
  text-align: center;
  margin-bottom: 40px;
  padding: 40px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  color: white;
}

.hero h1 {
  font-size: 42px;
  margin: 0 0 10px 0;
  font-weight: 800;
}

.hero p {
  font-size: 18px;
  margin: 0;
  opacity: 0.9;
}

.loading, .no-results {
  text-align: center;
  padding: 60px 20px;
  font-size: 18px;
  color: #7f8c8d;
}

.news-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 25px;
}

@media (max-width: 768px) {
  .hero h1 {
    font-size: 32px;
  }
  
  .news-grid {
    grid-template-columns: 1fr;
  }
}
</style>