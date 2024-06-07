<template>
    <li class="card">
      <div class="image">
        <img :src="character.image" :alt="character.name">
      </div>
      <h2>{{ character.name }}</h2>
      <div class="status">
        <span :class="character.status === 'Alive' ? 'alive' : character.status === 'Dead' ? 'dead' : 'unknown'"></span>
        <span>{{ character.status }} - {{ character.species }}</span>
      </div>
      <div class="information">
        <span>Gender: {{ character.gender }}</span>
        <span>Origin: {{ character.origin.name }}</span>
        <span>Location: {{ character.location.name }}</span>
      </div>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      character: Object,
    },
  };
  </script>
  
  <style scoped>
  .card {
    width: 20%;
    margin: 1%;
    border-radius: 1rem;
    background-color: var(--background-card);
    box-sizing: border-box;
    text-align: center;
    overflow: hidden;
    transition: transform 200ms ease-in-out;
    cursor: pointer;
  }
  
  .card:hover {
    transform: scale(1.05);
  }
  
  .card .image {
    width: 100%;
  }
  
  .card .image > img {
    width: 100%;
  }
  
  .card > h2 {
    margin: 5px;
    color:black;
  }
  
  .status {
    margin: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
  }
  
  .status span {
    color: var(--text-gray);
  }
  
  .status span:first-child {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    margin-right: 0.5rem;
  }
  
  .alive {
    background-color: green;
  }
  
  .dead {
    background-color: red;
  }
  
  .unknown {
    background-color: white;
  }
  
  .information {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  
  .information > span {
    color: var(--text-gray);
    margin: 2px;
  }
  
  .information > span:last-child {
    margin-bottom: 20px;
  }
  </style>
  