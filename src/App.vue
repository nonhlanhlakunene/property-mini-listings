<template>
  <div id="app">
    <header class="main-header">
      <div class="header-container">
        <h1>Homes & Beyond</h1>
        <span class="counter">Showing {{ filteredProperties.length }} Listings</span>
      </div>
    </header>

    <main class="container">
      <section class="controls">
        <input 
          v-model="searchQuery" 
          type="text" 
          placeholder="Search by title or location..." 
          class="search-input"
        />
        
        <select v-model="sortBy" class="sort-dropdown">
          <option value="default">Sort by Price</option>
          <option value="low-high">Price: Low to High</option>
          <option value="high-low">Price: High to Low</option>
        </select>
      </section>

      <section class="grid-container">
        <PropertyCard 
          v-for="prop in filteredProperties" 
          :key="prop.id" 
          :property="prop"
          @toggle-bookmark="handleBookmarkToggle"
        />
      </section>
      
      <div v-if="filteredProperties.length === 0" class="no-results">
        No properties match your criteria. Try searching for something else!
      </div>
    </main>
  </div>
</template>

<script>
import { mockProperties } from './properties.js';
import PropertyCard from './components/PropertyCard.vue';

export default {
  name: 'App',
  components: {
    PropertyCard
  },
  data() {
    return {
      properties: mockProperties,
      searchQuery: '',
      sortBy: 'default'
    };
  },
  computed: {
    filteredProperties() {
      // 1. First, handle searching by title or location (case-insensitive)
      let result = this.properties.filter(item => {
        const query = this.searchQuery.toLowerCase();
        return item.title.toLowerCase().includes(query) || 
               item.location.toLowerCase().includes(query);
      });

      // 2. Next, handle sorting on that filtered outcome
      if (this.sortBy === 'low-high') {
        result.sort((a, b) => a.price - b.price);
      } else if (this.sortBy === 'high-low') {
        result.sort((a, b) => b.price - a.price);
      }

      return result;
    }
  },
  methods: {
    handleBookmarkToggle(id) {
      const target = this.properties.find(p => p.id === id);
      if (target) {
        target.isBookmarked = !target.isBookmarked;
      }
    }
  }
}
</script>

<style>
/* Global Resets & Layout Layout */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
body {
  background-color: #f9f9f9;
  color: #333;
}
.main-header {
  background-color: #2c3e50;
  color: white;
  padding: 1.5rem 0;
  margin-bottom: 2rem;
}
.header-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 1rem;
}
.counter {
  background-color: #34495e;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.9rem;
}
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem 3rem 1rem;
}
.controls {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}
.search-input {
  flex-grow: 1;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
}
.sort-dropdown {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: white;
  font-size: 1rem;
  cursor: pointer;
}
/* Responsive Grid Framework */
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
}
.no-results {
  text-align: center;
  font-size: 1.2rem;
  color: #7f8c8d;
  margin-top: 3rem;
}
</style>