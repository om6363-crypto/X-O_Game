<!-- PlayerDashboard.vue -->
<template>
    <div id="app">
      <nav class="navbar navbar-expand-lg bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">Sportz Interactive</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                  data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                  aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
              <select v-model="selectedTeam" @change="filterPlayers">
                <option value="ALL">ALL</option>
                <option value="IND">IND</option>
                <option value="PAK">PAK</option>
                <option value="AUS">AUS</option>
                <option value="ENG">ENG</option>
              </select>
            </ul>
            <form class="d-flex" role="search">
              <input class="form-control me-2" type="search" placeholder="Search" v-model="searchQuery">
              <button class="btn btn-outline-success" type="button" @click="searchPlayers">Search</button>
            </form>
          </div>
        </div>
      </nav>
  
      <div class="container text-center">
        <div v-for="(players, role) in groupedPlayers" :key="role" class="row">
          <h1>{{ getRoleName(role) }}</h1>
          <div v-for="player in players" :key="player.name" class="col-md-4 mb-4">
            <div class="card">
              <img :src="player.image" class="card-img-top" alt="Player Image">
              <div class="card-body">
                <h5 class="card-title">{{ player.name }}</h5>
                <p class="card-text">Matches: {{ player.matches }}</p>
                <p class="card-text">Runs: {{ player.runs }}</p>
                <p class="card-text">50/100s: {{ player['50s'] }}/{{ player['100s'] }}</p>
                <p class="card-text">Highest Score: {{ player.highest_score }}</p>
                <p class="card-text">Team Name: {{ player.team_name }}</p>
                <p class="card-text">Best Bowling Figures: {{ player.best_bowling_figures || 'N/A' }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import playersData from '../assets/players.json';
  
  export default {
    name: 'PlayerDashboard',
    data() {
      return {
        players: [],
        selectedTeam: 'ALL',
        searchQuery: '',
        filteredPlayers: []
      };
    },
    computed: {
      groupedPlayers() {
        const grouped = {
          2: [], //batsmen
          3: [], // all-rounders
          4: []  // bowlers
        };
        this.filteredPlayers.forEach(player => {
          grouped[player.role].push(player);
        });
        return grouped;
      }
    },
    methods: {
      loadPlayers() {
        this.players = playersData.originalPlayers;
        this.filteredPlayers = this.players;
      },
      filterPlayers() {
        if (this.selectedTeam === 'ALL') {
          this.filteredPlayers = this.players;
        } else {
          this.filteredPlayers = this.players.filter(player => player.team_name === this.selectedTeam);
        }
      },
      searchPlayers() {
        const query = this.searchQuery.toLowerCase();
        this.filteredPlayers = this.players.filter(player => 
          player.name.toLowerCase().includes(query) ||
          player.team_name.toLowerCase().includes(query)
        );
      },
      getRoleName(role) {
        const roles = {
          2: 'Batsmen',
          3: 'All-rounders',
          4: 'Bowlers'
        };
        return roles[role] || 'Unknown';
      }
    },
    created() {
      this.loadPlayers();
    }
  };
  </script>