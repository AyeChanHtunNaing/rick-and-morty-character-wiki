<template>
    <input type="text" placeholder="Search by name" v-model="searchTerm" @input="filterByName">
  </template>
  
  <script>
  export default {
    data() {
      return {
        searchTerm: '',
      };
    },
    methods: {
      filterByName() {
        this.$emit('filter-name', this.searchTerm);
      },
    },
  };
  </script>
  
  <style scoped>
  input {
    width: 25%;
    padding: 0.9rem;
    border-radius: 0.5rem;
    border: none;
    margin-bottom: 1rem;
  }
  </style>
  