<template>
  <div>
    <div class="header">
      <h1>Administrateur</h1>
    </div>

    <div class="actions">
      <button @click="showSection('utilisateurs')">Gérer Utilisateurs</button>
      <button @click="showSection('questionnaires')">Gérer Questionnaires</button>
      <button @click="showSection('dashboard')">Dashboard</button>
    </div>

    <div v-if="currentSection === 'utilisateurs'" class="section-container">
      <h2>Liste des Utilisateurs</h2>
      <table class="data-table">
        <thead>
          <tr>
            <th>Choisir</th>
            <th>Nom d'utilisateur</th>
            <th>Nom</th>
            <th>E-mail</th>
            <th>Groupe</th>
            <th>Rôles</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(utilisateur, index) in utilisateurs" :key="index">
            <td><input type="checkbox" v-model="selectedUsers" :value="utilisateur.id" /></td>
            <td>{{ utilisateur.pseudo }}</td>
            <td>{{ utilisateur.nom }}</td>
            <td>{{ utilisateur.email }}</td>
            <td>{{ utilisateur.appartenir[0].groupe.nom }}</td>
            <td>{{ utilisateur.appartenir[0].groupe.role }}</td>
            <td>
              <button @click="editUser(utilisateur.id)">Modifier</button>
              <button @click="deleteUser(utilisateur.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-if="currentSection === 'questionnaires'" class="section-container">
      <h2>Liste des QCM</h2>
      <table class="data-table">
        <thead>
          <tr>
            <th>Nom du QCM</th>
            <th>Catégorie</th>
            <th>Groupe</th>
            <th>Date de création</th>
            <th>Status</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(qcm, index) in questionnaires" :key="index">
            <td>{{ qcm.nom }}</td>
            <td>{{ qcm.categorie }}</td>
            <td>{{ qcm.groupe }}</td>
            <td>{{ qcm.date_creation }}</td>
            <td>{{ qcm.status }}</td>
            <td>
              <button @click="editQcm(qcm.id)">Modifier</button>
              <button @click="deleteQcm(qcm.id)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div v-if="currentSection === 'dashboard'" class="section-container">
      <h2>Dashboard</h2>
      <p>Bienvenue sur votre tableau de bord administrateur.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { supabase } from '../supabase';

const currentSection = ref('dashboard');
const utilisateurs = ref([]);
const selectedUsers = ref([]);
const questionnaires = ref([]);

const fetchUtilisateurs = async () => {
  const { data, error } = await supabase
    .from('utilisateur')
    .select(`*, appartenir (groupe (nom, role))`);

  if (error) {
    console.error('Erreur lors de la récupération des utilisateurs:', error);
  } else {
    utilisateurs.value = data;
  }
};

const fetchQuestionnaires = async () => {
  const { data, error } = await supabase.from('questionnaires').select('*');

  if (error) {
    console.error('Erreur lors de la récupération des questionnaires:', error);
  } else {
    questionnaires.value = data;
  }
};

onMounted(() => {
  fetchUtilisateurs();
  fetchQuestionnaires();
});

const showSection = (section) => {
  currentSection.value = section;
};

const editUser = (userId) => {
  console.log('Modifier l\'utilisateur', userId);
};

const deleteUser = (userId) => {
  console.log('Supprimer l\'utilisateur', userId);
};

const editQcm = (qcmId) => {
  console.log('Modifier le QCM', qcmId);
};

const deleteQcm = (qcmId) => {
  console.log('Supprimer le QCM', qcmId);
};
</script>

<style scoped>
.header {
  background-color: #c59edb;
  width: 100%;
  text-align: center;
  padding: 20px;
  position: fixed;
  top: 0;
  left: 0;
}

.header h1 {
  margin: 0;
  color: white;
  font-size: 2rem;
}

.actions {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 100px;
}

button {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #0056b3;
}

.section-container {
  margin-top: 30px;
  padding: 20px;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  text-align: left;
}

.data-table th,
.data-table td {
  padding: 12px;
  border: 1px solid #ddd;
}

.data-table th {
  background-color: #f4f4f4;
}

.data-table td {
  background-color: #f9f9f9;
}

button {
  background-color: #c59edb;
  border: none;
  padding: 10px 20px;
  color: white;
  font-size: 1rem;
  border-radius: 5px;
  cursor: pointer;
  margin-left:5px;
}

button:hover {
  background-color: #b48ac6;
}
</style>
