<template>
  <div class="category-filter">
    <button 
      v-for="category in categories" 
      :key="category"
      @click="$emit('select', category)"
      :class="['category-btn', { active: category === selectedCategory }]"
    >
      {{ category }}
    </button>
  </div>
</template>

<script>
export default {
  name: 'CategoryFilter',
  props: {
    selectedCategory: {
      type: String,
      default: 'Semua'
    }
  },
  data() {
    return {
      categories: ['Semua', 'Teknologi', 'Olahraga', 'Hiburan', 'Politik', 'Bisnis', 'Kesehatan', 'Gaya Hidup', 'Kuliner', 'Pendidikan']
    }
  }
}
</script>

<style scoped>
.category-filter {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding: 10px 0;
  margin-bottom: 30px;
}

.category-filter::-webkit-scrollbar {
  height: 6px;
}

.category-filter::-webkit-scrollbar-thumb {
  background: #bdc3c7;
  border-radius: 3px;
}

.category-btn {
  background: white;
  border: 2px solid #ecf0f1;
  color: #7f8c8d;
  padding: 10px 20px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  white-space: nowrap;
  transition: all 0.3s ease;
}

.category-btn:hover {
  border-color: #3498db;
  color: #3498db;
}

.category-btn.active {
  background: #3498db;
  border-color: #3498db;
  color: white;
}
</style>