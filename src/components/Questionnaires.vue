<template>
    <div class="questionnaires-container">
      <h2>Questionnaires</h2>
      <table class="questionnaires-table">
        <thead>
          <tr>
            <th>Nom</th>
            <th>Catégorie</th>
            <th>Groupe</th>
            <th>Date de création</th>
            <th>Status</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="questionnaire in questionnaires" :key="questionnaire.id">
            <td>{{ questionnaire.nom }}</td>
            <td>{{ questionnaire.categorie }}</td>
            <td>{{ questionnaire.groupe }}</td>
            <td>{{ questionnaire.date_creation }}</td>
            <td>{{ questionnaire.status }}</td>
            <td><button>Éditer</button> <button>Supprimer</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const questionnaires = ref([]);
  
  const fetchQuestionnaires = async () => {
    questionnaires.value = [
      { id: 1, nom: 'QCM PHP', categorie: 'Développement', groupe: 'Equipe Dev', date_creation: '2024-01-15', status: 'Actif' },
      // Ajouter les autres questionnaires ici
    ];
  };
  
  onMounted(fetchQuestionnaires);
  </script>
  
  <style scoped>
  .questionnaires-container {
    padding: 20px;
  }
  
  .questionnaires-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .questionnaires-table th, .questionnaires-table td {
    padding: 10px;
    border: 1px solid #ddd;
  }
  
  .questionnaires-table th {
    background-color: #f0f0f0;
  }
  </style>
  