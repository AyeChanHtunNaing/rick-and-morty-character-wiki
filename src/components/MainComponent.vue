<template>
    <section>
      <h1>Rick and Morty <br> Characters</h1>
  
      <FilterComponent :activeFilter="activeFilter" @filter-status="filterByStatus" />
  
      <SearchComponent @filter-name="filterByName" />
  
      <div v-if="filteredCharacters.length > 0" class="container-cards">
        <ul class="card-list">
          <CharacterCardComponent v-for="character in filteredCharacters" :key="character.id" :character="character" />
        </ul>
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
      };
    },
    mounted() {
      this.fetchCharacters('https://rickandmortyapi.com/api/character/')
        .then(() => {
          this.filteredCharacters = this.characters;
        });
    },
    methods: {
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
      },
      filterByName(searchTerm) {
        this.filteredCharacters = this.characters.filter(character => character.name.toLowerCase().includes(searchTerm.toLowerCase()));
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
  