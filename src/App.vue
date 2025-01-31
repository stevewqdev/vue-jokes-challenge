// JokesApp.vue
<template>
  <div class="max-[1024px]:min-h-[auto] max-[1024px]:py-20 container flex flex-col min-h-[100vh] justify-center items-center py-20">
    <h1 class="max-[400px]:text-4xl text-5xl font-bold">Joke Randomizer</h1>
    <p class="max-[400px]:max-w-[300px] text-sm mt-3 max-w-[400px] opacity-50">Everytime you refresh the page you will get a whole batch of new jokes to share with your friends</p>
    <div class="controls mt-5 border px-5 py-3 rounded-lg">
      <label class="font-bold text-sm" for="sort">Sort by:</label>
      <select class="text-sm" v-model="sortOption" @change="sortJokes">
        <option value="">All Categories</option>
        <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
      </select>
    </div>
    <ul v-if="paginatedJokes.length" class="max-[1024px]:gap-3 max-[1024px]:px-10 flex flex-wrap justify-center px-20 gap-5 w-full">
      <li v-for="joke in paginatedJokes" :key="joke.id" class="max-[768px]:mt-5 max-[768px]:max-w-[100%] max-[1024px]:max-w-[49%] relative mt-5 shadow-lg py-12 px-10 rounded-lg bg-white w-full max-w-[32%] flex justify-center items-center flex-col bg-[#f3f6ff]">
        <p class="text-xl italic max-w-[300px]"><strong>{{ joke.setup }}</strong></p>
        <p class="text-md mt-2">{{ joke.punchline }} 🥁</p>
        <small class="absolute bottom-[1rem] right-[1rem] font-bold capitalize text-xs">{{ joke.type }}</small>
      </li>
    </ul>
    <p class="text-md mt-10" v-else>Sadly, there are no jokes in the selected category. Try another one.</p>
    <div class="pagination mt-20 flex gap-5" v-if="totalPages > 1">
      <button @click="prevPage" :disabled="currentPage === 1" class="text-sm font-bold disabled:opacity-50">Previous</button>
      <span class="text-sm">Page <span class="font-bold">{{ currentPage }}</span> of <span class="font-bold">{{ totalPages }}</span></span>
      <button @click="nextPage" :disabled="currentPage >= totalPages" class="text-sm font-bold disabled:opacity-50">Next</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      jokes: [],
      categories: [],
      sortOption: '',
      currentPage: 1,
      perPage: 5,
    };
  },
  computed: {
    sortedJokes() {
      if (!this.sortOption) return this.jokes;
      return [...this.jokes].filter(joke => joke.type === this.sortOption);
    },
    paginatedJokes() {
      const start = (this.currentPage - 1) * this.perPage;
      return this.sortedJokes.slice(start, start + this.perPage);
    },
    totalPages() {
      return Math.ceil(this.sortedJokes.length / this.perPage);
    }
  },
  methods: {
    async fetchJokes() {
      try {
        const response = await axios.get('https://official-joke-api.appspot.com/jokes/random/250');
        this.jokes = response.data;
      } catch (error) {
        console.error('Error fetching jokes:', error);
      }
    },
    async fetchCategories() {
      try {
        const response = await axios.get('https://official-joke-api.appspot.com/types');
        this.categories = response.data;
      } catch (error) {
        console.error('Error fetching categories:', error);
      }
    },
    sortJokes() {
      this.currentPage = 1;
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    }
  },
  mounted() {
    this.fetchJokes();
    this.fetchCategories();
  }
};
</script>

<style>
.container {
  width: 50%;
  margin: auto;
  text-align: center;
}
.controls {
  margin-bottom: 20px;
}
.pagination {
  margin-top: 20px;
}
</style>
