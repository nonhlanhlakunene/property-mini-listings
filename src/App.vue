<template>
  <div id="app">

    <header class="main-header">
      <h1>🏡 Homes & Beyond</h1>
      <p>{{ filteredProperties.length }} properties found</p>
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

      <section v-if="filteredProperties.length === 0" class="no-results">
        <p>No properties match your search. Try a different keyword.</p>
      </section>

      <section class="listings-grid">
        <PropertyCard
          v-for="property in filteredProperties"
          :key="property.id"
          :property="property"
          :bookmarkedIds="bookmarkedIds"
          @toggle-bookmark="handleBookmark"
        />
      </section>

    </main>

  </div>
</template>

<script>
import PropertyCard from './components/PropertyCard.vue'
import properties from './properties.js'

export default {
  name: 'App',

  components: {
    PropertyCard
  },

  data: function () {
    return {
      properties: properties,
      searchQuery: '',
      sortBy: 'default',
      bookmarkedIds: []
    }
  },

  computed: {
    filteredProperties: function () {
      var query = this.searchQuery.toLowerCase()

      var result = this.properties.filter(function (property) {
        return (
          property.title.toLowerCase().includes(query) ||
          property.location.toLowerCase().includes(query)
        )
      })

      if (this.sortBy === 'low-high') {
        result = result.slice().sort(function (a, b) {
          return a.price - b.price
        })
      } else if (this.sortBy === 'high-low') {
        result = result.slice().sort(function (a, b) {
          return b.price - a.price
        })
      }

      return result
    }
  },

  methods: {
    handleBookmark: function (id) {
      var index = this.bookmarkedIds.indexOf(id)
      if (index === -1) {
        this.bookmarkedIds.push(id)
      } else {
        this.bookmarkedIds.splice(index, 1)
      }
    }
  }
}
</script>