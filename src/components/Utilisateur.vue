<template>
    <div class="utilisateur-container">
      <h2>Utilisateur</h2>
      <div class="search-bar">
        <input type="text" v-model="searchQuery" placeholder="Rechercher un utilisateur" />
        <button @click="addUser">Ajouter un utilisateur</button>
      </div>
      <table class="utilisateur-table">
        <thead>
          <tr>
            <th><input type="checkbox" @change="toggleAll" /></th>
            <th>Utilisateur</th>
            <th>Nom</th>
            <th>Email</th>
            <th>Groupe</th>
            <th>Rôles</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in filteredUsers" :key="user.id">
            <td><input type="checkbox" v-model="user.selected" /></td>
            <td>{{ user.utilisateur }}</td>
            <td>{{ user.nom }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.groupe }}</td>
            <td>{{ user.roles }}</td>
            <td><button>Éditer</button> <button>Supprimer</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const users = ref([
    { id: 1, utilisateur: 'Werenoi', nom: 'Pierre', email: 'pierre@mail.com', groupe: 'Equipe Dev', roles: 'admin', selected: false },
    // Ajouter les autres utilisateurs ici
  ]);
  
  const searchQuery = ref('');
  
  const filteredUsers = computed(() =>
    users.value.filter(user =>
      user.nom.toLowerCase().includes(searchQuery.value.toLowerCase())
    )
  );
  
  const addUser = () => {
    console.log('Ajouter un nouvel utilisateur');
  };
  
  const toggleAll = (event) => {
    const checked = event.target.checked;
    users.value.forEach(user => (user.selected = checked));
  };
  </script>
  
  <style scoped>
  .utilisateur-container {
    padding: 20px;
  }
  
  .search-bar {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }
  
  .utilisateur-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .utilisateur-table th, .utilisateur-table td {
    padding: 10px;
    border: 1px solid #ddd;
  }
  
  .utilisateur-table th {
    background-color: #f0f0f0;
  }
  </style>
  