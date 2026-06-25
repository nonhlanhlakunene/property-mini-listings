<template>
  <div class="card" :class="{ unavailable: !property.available }">
    <div class="card-image-wrapper">
      <img :src="property.image" :alt="property.title" class="card-image" />
      <span v-if="!property.available" class="ribbon">Not Available</span>
      <button class="bookmark-btn" @click="toggleBookmark">
        {{ isBookmarked ? "🔖" : "🤍" }}
      </button>
    </div>

    <div class="card-body">
      <span class="property-type">{{ property.type }}</span>
      <h3 class="card-title">{{ property.title }}</h3>
      <p class="card-location">📍 {{ property.location }}</p>
      <p class="card-price">R {{ property.price.toLocaleString() }} / month</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "PropertyCard",

  props: {
    property: {
      type: Object,
      required: true,
    },
    bookmarkedIds: {
      type: Array,
      required: true,
    },
  },

  emits: ["toggle-bookmark"],

  computed: {
    isBookmarked: function () {
      return this.bookmarkedIds.includes(this.property.id);
    },
  },

  methods: {
    toggleBookmark: function () {
      this.$emit("toggle-bookmark", this.property.id);
    },
  },
};
</script>
