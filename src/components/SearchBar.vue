<template>
  <div class="search-bar">
    <input 
      type="text" 
      v-model="searchQuery"
      @input="onSearch"
      placeholder="Cari berita..." 
      class="search-input"
    />
    <span class="search-icon">🔍</span>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  data() {
    return {
      searchQuery: ''
    }
  },
  methods: {
    onSearch() {
      this.$emit('search', this.searchQuery);
    }
  }
}
</script>

<style scoped>
.search-bar {
  position: relative;
  margin-bottom: 30px;
}

.search-input {
  width: 100%;
  padding: 15px 50px 15px 20px;
  border: 2px solid #ecf0f1;
  border-radius: 30px;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

.search-input:focus {
  outline: none;
  border-color: #3498db;
}

.search-icon {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 20px;
}
</style>