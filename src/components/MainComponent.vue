<template>
  <section>
    <h1>Rick and Morty <br> Characters</h1>

    <FilterComponent :activeFilter="activeFilter" @filter-status="filterByStatus" />
    <SearchComponent @filter-name="filterByName" />

    <div v-if="loading" class="loading-indicator">
      <p>Loading... ({{ countdown }} seconds remaining)</p>
    </div>

    <div v-if="!loading && filteredCharacters.length > 0" class="container-cards">
      <ul class="card-list">
        <CharacterCardComponent 
          v-for="character in paginatedCharacters" 
          :key="character.id" 
          :character="character" 
        />
      </ul>
    </div>

    <div v-if="!loading" class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </section>
</template>

<script>
import FilterComponent from './FilterComponent.vue';
import SearchComponent from './SearchComponent.vue';
import CharacterCardComponent from './CharacterCardComponent.vue';
import axios from 'axios';

export default {
  components: {
    FilterComponent,
    SearchComponent,
    CharacterCardComponent,
  },
  data() {
    return {
      characters: [],
      filteredCharacters: [],
      searchTerm: '',
      activeFilter: 'All',
      currentPage: 1,
      itemsPerPage: 20,
      loading: true,
      countdown: 30,
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.filteredCharacters.length / this.itemsPerPage);
    },
    paginatedCharacters() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.filteredCharacters.slice(start, end);
    },
  },
  mounted() {
    this.startCountdown();
    this.fetchCharacters('https://rickandmortyapi.com/api/character/')
      .then(() => {
        this.filteredCharacters = this.characters;
        this.loading = false;
      });
  },
  methods: {
    startCountdown() {
      const countdownInterval = setInterval(() => {
        if (this.countdown > 0) {
          this.countdown--;
        } else {
          clearInterval(countdownInterval);
        }
      }, 1000);
    },
    fetchCharacters(url) {
      return new Promise((resolve, reject) => {
        axios.get(url)
          .then(response => {
            this.characters = this.characters.concat(response.data.results);

            if (response.data.info.next) {
              this.fetchCharacters(response.data.info.next).then(resolve);
            } else {
              resolve();
            }
          })
          .catch(error => {
            console.error('Error fetching character list:', error);
            reject(error);
          });
      });
    },
    filterByStatus(status) {
      const lowerCaseStatus = status.toLowerCase();
      this.activeFilter = status;
      if (lowerCaseStatus === 'all') {
        this.filteredCharacters = this.characters;
      } else {
        this.filteredCharacters = this.characters.filter(character => character.status.toLowerCase() === lowerCaseStatus);
      }
      this.currentPage = 1; 
    },
    filterByName(searchTerm) {
      this.filteredCharacters = this.characters.filter(character => character.name.toLowerCase().includes(searchTerm.toLowerCase()));
      this.currentPage = 1; 
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
  },
};
</script>

<style scoped>
section {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 0 10rem;
  width: 100vh;
}

h1 {
  text-align: center;
  font-size: 45px;
  margin-top: 4rem;
}

.loading-indicator {
  font-size: 20px;
  text-align: center;
  margin-top: 20px;
}

.container-cards {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 2rem;
}

.pagination button {
  padding: 0.5rem 1rem;
  margin: 0 0.5rem;
  cursor: pointer;
}

.pagination button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

@media only screen and (min-width: 370px) and (max-width: 600px) {
  section {
    width: 100%;
    margin: 0;
  }
  h1 {
    font-size: 35px;
    margin-top: 1rem;
  }
  .filter {
    width: 80%;
  }
  input {
    width: 80%;
  }
  .card {
    width: 80%;
    margin-bottom: 1rem;
  }
}

@media only screen and (min-width: 601px) and (max-width: 768px) {
  section {
    width: 100%;
    margin: 0;
  }
  h1 {
    font-size: 35px;
    margin-top: 1rem;
  }
  .filter {
    width: 86%;
  }
  input {
    width: 86%;
  }
  .card {
    width: 40%;
    margin-bottom: 1rem;
  }
}

@media only screen and (min-width: 769px) and (max-width: 1024px) {
  section {
    width: 100%;
    margin: 0;
  }
  h1 {
    font-size: 45px;
    margin-top: 4rem;
  }
  .filter {
    width: 40%;
  }
  input {
    width: 40%;
  }
  .card {
    width: 30%;
    margin-bottom: 1rem;
  }
}

@media only screen and (min-width: 1025px) and (max-width: 1201px) {
  section {
    width: 100%;
    margin: 0;
  }
  h1 {
    font-size: 45px;
    margin-top: 4rem;
  }
  .filter {
    width: 45%;
  }
  input {
    width: 45%;
  }
  .card {
    width: 22%;
    margin-bottom: 1rem;
  }
}
</style>
