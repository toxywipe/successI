<template>
  <div>
    <!-- Header de la page Questionnaires -->
    <header>
      <h1>Gestion des Questionnaires</h1>
    </header>

    <section>
      <h2>Gérer les Questionnaires</h2>
      <button @click="showCreationForm">Créer un Questionnaire</button>

      <!-- Section pour ajouter un questionnaire -->
      <div v-if="showCreation">
        <!-- Composant pour créer un questionnaire -->
        <CreationQ @refresh="fetchQuestionnaires" @cancel="hideCreationForm" />
      </div>

      <table v-else class="data-table">
        <thead>
          <tr>
            <th>Titre</th>
            <th>Date de création</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(questionnaire, index) in questionnaires" :key="index">
            <td>{{ questionnaire.titre }}</td>
            <td>{{ questionnaire.date_creation }}</td>
            <td>
              <button @click="editQuestionnaire(questionnaire.id)">Modifier</button>
              <button @click="deleteQuestionnaire(questionnaire.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { supabase } from '../supabase';
import CreationQ from './CreationQ.vue';

const questionnaires = ref([]);
const showCreation = ref(false);

const fetchQuestionnaires = async () => {
  const { data, error } = await supabase.from('questionnaire').select('*');
  if (error) {
    console.error('Erreur lors de la récupération des questionnaires:', error);
  } else {
    questionnaires.value = data;
  }
};

const showCreationForm = () => {
  showCreation.value = true;
};

const hideCreationForm = () => {
  showCreation.value = false;
};

const editQuestionnaire = (questionnaireId) => {
  console.log('Modifier le questionnaire', questionnaireId);
};

const deleteQuestionnaire = (questionnaireId) => {
  console.log('Supprimer le questionnaire', questionnaireId);
};

onMounted(() => {
  fetchQuestionnaires();
});
</script>

<style scoped>
header {
  background-color: #c59edb;
  color: white;
  padding: 20px;
  text-align: center;
}

button {
  background-color: #c59edb;
  border: none;
  padding: 10px;
  color: white;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #b48ac6;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

table th, table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: left;
}

.data-table th {
  background-color: #f0f0f0;
}

.data-table tr:nth-child(even) {
  background-color: #f9f9f9;
}
</style>
