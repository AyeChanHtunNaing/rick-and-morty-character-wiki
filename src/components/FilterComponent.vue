<template>
    <div class="filter">
      <div class="item" :class="{ active: activeFilter === 'All' }" @click="filterByStatus('All')">All</div>
      <div class="item" :class="{ active: activeFilter === 'Alive' }" @click="filterByStatus('Alive')">Alive</div>
      <div class="item" :class="{ active: activeFilter === 'Dead' }" @click="filterByStatus('Dead')">Dead</div>
      <div class="item" :class="{ active: activeFilter === 'Unknown' }" @click="filterByStatus('Unknown')">Unknown</div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      activeFilter: String,
    },
    methods: {
      filterByStatus(status) {
        this.$emit('filter-status', status);
      },
    },
  };
  </script>
  
  <style scoped>

  .filter {
    width: 25%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    background-color: var(--background-card);
    padding: 0.9rem;
    font-size: 15px;
    border-radius: 0.5rem;
    cursor: pointer;
    margin-bottom: 1rem;
    color:silver;
  }
  
  .filter .item.active {
    color: orange;
  }
  </style>
  