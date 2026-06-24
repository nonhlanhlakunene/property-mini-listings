<template>
  <div class="property-card" :class="{ 'unavailable-opacity': !property.available }">
    <div class="image-container">
      <img :src="property.image" :alt="property.title" class="property-image" />
      <div v-if="!property.available" class="availability-badge">
        Not Available
      </div>
    </div>

    <div class="property-details">
      <span class="property-type">{{ property.type }}</span>
      <h3>{{ property.title }}</h3>
      <p class="location">📍 {{ property.location }}</p>
      
      <div class="card-footer">
        <span class="price">R {{ property.price }} <small>/ night</small></span>
        
        <button 
          @click="$emit('toggle-bookmark', property.id)" 
          class="bookmark-btn"
          :class="{ 'is-bookmarked': property.isBookmarked }"
        >
          {{ property.isBookmarked ? '★ Bookmarked' : '☆ Bookmark' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PropertyCard',
  props: {
    property: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.property-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  position: relative;
}
.unavailable-opacity {
  border-color: #ffcccc;
}
.image-container {
  position: relative;
  height: 200px;
  width: 100%;
}
.property-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.availability-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #e74c3c;
  color: white;
  padding: 4px 8px;
  font-size: 0.75rem;
  font-weight: bold;
  border-radius: 4px;
  text-transform: uppercase;
}
.property-details {
  padding: 1rem;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}
.property-type {
  font-size: 0.8rem;
  text-transform: uppercase;
  color: #7f8c8d;
  font-weight: bold;
}
h3 {
  margin: 0.5rem 0;
  font-size: 1.2rem;
  color: #2c3e50;
}
.location {
  color: #95a5a6;
  font-size: 0.9rem;
  margin-bottom: 1rem;
}
.card-footer {
  margin-top: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.price {
  font-weight: bold;
  font-size: 1.1rem;
  color: #2ecc71;
}
.bookmark-btn {
  background: none;
  border: 1px solid #34495e;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}
.bookmark-btn.is-bookmarked {
  background: #f1c40f;
  border-color: #f1c40f;
  color: #000;
}
</style>