<script>
import Catalog from './components/Catalog.vue';
import Rentals from './components/Rentals.vue';

export default {
  data() {
    return {
      rental: 1
    }
  },
  components: {
    Rentals,
    Catalog
  }
}
</script>

<template>
  <div class="container">
    <ul class="nav nav-tabs">
      <li class="nav-item">
        <a class="nav-link" :class="rental == 1 ? 'active' : ''" @click="rental = 1" href="#">Rentals</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" :class="rental == 0 ? 'active' : ''" @click="rental = 0" href="#">Catalogs</a>
      </li>
    </ul>

    <div v-if="rental == 1">
      <Rentals />
    </div>
    <div v-else>
      <Catalog />
    </div>
  </div>

</template>

<style scoped></style>
