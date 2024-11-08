<template>
  <div>
    <header>
      <h1>Gestion des Questionnaires</h1>
    </header>

    <section>
      <button @click="showCreationForm">Créer un Questionnaire</button>

      <div v-if="showCreation">
        <CreationQ @refresh="fetchQuestionnaires" @cancel="hideCreationForm" />
      </div>

      <table v-else class="data-table">
        <thead>
          <tr>
            <!-- Colonne "Tout cocher" -->
            <th>
              <input type="checkbox" v-model="selectAll" @change="toggleSelectAll" />
            </th>
            <th>Nom</th>
            <th>Date de création</th>
            <th>Temps</th>
            <th>Code</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(questionnaire, index) in questionnaires" :key="questionnaire.id">
            <td>
              <!-- Case à cocher pour chaque questionnaire -->
              <input 
                type="checkbox" 
                :checked="selectedQuestionnaires.includes(questionnaire.id)"
                @change="toggleQuestionnaireSelection(questionnaire.id)"
              />
            </td>
            <td>{{ questionnaire.nom }}</td>
            <td>{{ questionnaire.date_creation }}</td>
            <td>{{ questionnaire.temps_de_passage }} minutes</td>
            <td>{{ questionnaire.code }}</td>
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
const selectAll = ref(false);  // Case "Tout cocher"
const selectedQuestionnaires = ref([]);  // Liste des questionnaires sélectionnés

// Récupérer les questionnaires depuis Supabase
const fetchQuestionnaires = async () => {
  const { data, error } = await supabase.from('questionnaire').select('*');
  if (error) {
    console.error('Erreur lors de la récupération des questionnaires:', error);
  } else {
    questionnaires.value = data;
  }
};

// Afficher le formulaire de création de questionnaire
const showCreationForm = () => {
  showCreation.value = true;
};

// Cacher le formulaire de création
const hideCreationForm = () => {
  showCreation.value = false;
};

// Fonction de modification du questionnaire
const editQuestionnaire = (questionnaireId) => {
  console.log('Modifier le questionnaire', questionnaireId);
};

// Fonction de suppression du questionnaire
const deleteQuestionnaire = (questionnaireId) => {
  console.log('Supprimer le questionnaire', questionnaireId);
};

// Gérer la sélection/dé-sélection de la case "Tout cocher"
const toggleSelectAll = () => {
  if (selectAll.value) {
    selectedQuestionnaires.value = questionnaires.value.map((q) => q.id); // Sélectionner tous
  } else {
    selectedQuestionnaires.value = []; // Désélectionner tous
  }
};

// Gérer la sélection/dé-sélection d'un questionnaire individuel
const toggleQuestionnaireSelection = (id) => {
  if (selectedQuestionnaires.value.includes(id)) {
    selectedQuestionnaires.value = selectedQuestionnaires.value.filter((qid) => qid !== id);  // Retirer
  } else {
    selectedQuestionnaires.value.push(id);  // Ajouter
  }

  // Vérifier si tous les questionnaires sont sélectionnés pour gérer la case "Tout cocher"
  selectAll.value = questionnaires.value.length === selectedQuestionnaires.value.length;
};

// Au montage du composant, récupérer les questionnaires
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
