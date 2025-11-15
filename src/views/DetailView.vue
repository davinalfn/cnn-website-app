<template>
  <div class="detail-view">
    <button @click="goBack" class="back-btn">← Kembali</button>

    <div v-if="loading" class="loading">
      <p>Memuat berita...</p>
    </div>

    <div v-else-if="!news" class="not-found">
      <h2>❌ Berita tidak ditemukan</h2>
      <button @click="goBack" class="btn-primary">Kembali ke Beranda</button>
    </div>

    <article v-else class="news-detail">
      <div class="news-header">
        <span class="category-badge">{{ news.category }}</span>
        <h1 class="news-title">{{ news.title }}</h1>
        <div class="news-meta">
          <span>✍️ {{ news.author }}</span>
          <span>📅 {{ formatDate(news.date) }}</span>
          <span>👁️ {{ news.views.toLocaleString() }} views</span>
        </div>
      </div>

      <div class="news-image">
        <img :src="news.image" :alt="news.title" />
      </div>

      <div class="news-content">
        <p class="news-summary">{{ news.summary }}</p>
        <p class="news-body">{{ news.content }}</p>
      </div>

      <div class="related-news" v-if="relatedNews.length > 0">
        <h3>📌 Berita Terkait</h3>
        <div class="related-grid">
          <NewsCard v-for="related in relatedNews" :key="related.id" :news="related" />
        </div>
      </div>
    </article>
  </div>
</template>

<script>
import NewsCard from '../components/NewsCard.vue';

export default {
  name: 'DetailView',
  components: {
    NewsCard
  },
  data() {
    return {
      news: null,
      allNews: [],
      relatedNews: [],
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
        
        const newsId = parseInt(this.$route.params.id);
        this.news = this.allNews.find(n => n.id === newsId);
        
        if (this.news) {
          // Get related news (same category, different id)
          this.relatedNews = this.allNews
            .filter(n => n.category === this.news.category && n.id !== this.news.id)
            .slice(0, 3);
        }
        
        this.loading = false;
      } catch (error) {
        console.error('Error loading news:', error);
        this.loading = false;
      }
    },
    formatDate(dateStr) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(dateStr).toLocaleDateString('id-ID', options);
    },
    goBack() {
      this.$router.push('/');
    }
  }
}
</script>

<style scoped>
.detail-view {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}

.back-btn {
  background: white;
  border: 2px solid #ecf0f1;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 30px;
  transition: all 0.3s ease;
}

.back-btn:hover {
  border-color: #3498db;
  color: #3498db;
}

.loading, .not-found {
  text-align: center;
  padding: 60px 20px;
}

.not-found h2 {
  color: #e74c3c;
  margin-bottom: 20px;
}

.btn-primary {
  background: #3498db;
  color: white;
  border: none;
  padding: 12px 30px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: background 0.3s ease;
}

.btn-primary:hover {
  background: #2980b9;
}

.news-detail {
  background: white;
  border-radius: 12px;
  padding: 40px;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
}

.news-header {
  margin-bottom: 30px;
}

.category-badge {
  display: inline-block;
  background: #e74c3c;
  color: white;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 20px;
}

.news-title {
  font-size: 36px;
  font-weight: 800;
  color: #2c3e50;
  margin: 0 0 20px 0;
  line-height: 1.3;
}

.news-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  font-size: 14px;
  color: #7f8c8d;
  padding: 15px 0;
  border-top: 2px solid #ecf0f1;
  border-bottom: 2px solid #ecf0f1;
}

.news-image {
  width: 100%;
  height: 450px;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 30px;
}

.news-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.news-content {
  line-height: 1.8;
  color: #34495e;
}

.news-summary {
  font-size: 20px;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 20px;
}

.news-body {
  font-size: 18px;
  text-align: justify;
}

.related-news {
  margin-top: 60px;
  padding-top: 40px;
  border-top: 3px solid #ecf0f1;
}

.related-news h3 {
  font-size: 24px;
  color: #2c3e50;
  margin-bottom: 25px;
}

.related-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
}

@media (max-width: 768px) {
  .news-detail {
    padding: 20px;
  }
  
  .news-title {
    font-size: 28px;
  }
  
  .news-image {
    height: 250px;
  }
  
  .related-grid {
    grid-template-columns: 1fr;
  }
}
</style>