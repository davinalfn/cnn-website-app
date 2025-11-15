<template>
  <div class="news-card">
    <router-link :to="`/berita/${news.id}`" class="news-link">
      <div class="news-image">
        <img :src="news.image" :alt="news.title" />
        <span class="category-badge">{{ news.category }}</span>
        <span v-if="news.trending" class="trending-badge">🔥 Trending</span>
      </div>
      <div class="news-content">
        <h3 class="news-title">{{ news.title }}</h3>
        <p class="news-summary">{{ news.summary }}</p>
        <div class="news-meta">
          <span class="author">✍️ {{ news.author }}</span>
          <span class="date">📅 {{ formatDate(news.date) }}</span>
          <span class="views">👁️ {{ formatViews(news.views) }}</span>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'NewsCard',
  props: {
    news: {
      type: Object,
      required: true
    }
  },
  methods: {
    formatDate(dateStr) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(dateStr).toLocaleDateString('id-ID', options);
    },
    formatViews(views) {
      if (views >= 1000) {
        return (views / 1000).toFixed(1) + 'k';
      }
      return views.toString();
    }
  }
}
</script>

<style scoped>
.news-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.news-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}

.news-link {
  text-decoration: none;
  color: inherit;
  display: block;
}

.news-image {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.news-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.news-card:hover .news-image img {
  transform: scale(1.05);
}

.category-badge {
  position: absolute;
  top: 12px;
  left: 12px;
  background: #e74c3c;
  color: white;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.trending-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  background: #f39c12;
  color: white;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
}

.news-content {
  padding: 20px;
}

.news-title {
  font-size: 18px;
  font-weight: 700;
  color: #2c3e50;
  margin: 0 0 12px 0;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.news-summary {
  font-size: 14px;
  color: #7f8c8d;
  margin: 0 0 16px 0;
  line-height: 1.6;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.news-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  font-size: 12px;
  color: #95a5a6;
}

.news-meta span {
  display: flex;
  align-items: center;
  gap: 4px;
}
</style>