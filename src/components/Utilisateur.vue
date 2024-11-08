<template>
  <div>
    <!-- Header de la page Utilisateur -->
    <header>
      <h1>Gestion des Utilisateurs</h1>
    </header>

    <section>
      <h2>Gérer les Utilisateurs</h2>
      <button @click="showCreationForm">Créer un utilisateur</button>

      <!-- Formulaire de création -->
      <div v-if="showCreation">
        <CreationU @refresh="fetchUtilisateurs" @cancel="hideCreationForm" />
      </div>

      <!-- Liste des utilisateurs -->
      <table v-else class="data-table">
        <thead>
          <tr>
            <th>Nom d'utilisateur</th>
            <th>Nom</th>
            <th>Email</th>
            <th>Groupe</th>
            <th>Options</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(utilisateur, index) in utilisateurs" :key="index">
            <td>{{ utilisateur.pseudo }}</td>
            <td>{{ utilisateur.nom }}</td>
            <td>{{ utilisateur.email }}</td>
            <td>{{ utilisateur.appartenir[0]?.groupe.nom || 'N/A' }}</td>
            <td>
              <button @click="editUser(utilisateur)">Modifier</button>
              <button @click="confirmDeleteUser(utilisateur)">Supprimer</button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>

    <!-- Modal pour modification d'utilisateur -->
    <div v-if="showEditModal" class="modal">
      <div class="modal-content">
        <h3>Modifier l'utilisateur</h3>
        <label for="editNom">Nom d'utilisateur:</label>
        <input type="text" v-model="editUtilisateur.pseudo" id="editNom" />

        <label for="editEmail">Email:</label>
        <input type="email" v-model="editUtilisateur.email" id="editEmail" />

        <button @click="updateUser">Mettre à jour</button>
        <button @click="closeEditModal">Annuler</button>
      </div>
    </div>

    <!-- Modal de confirmation de suppression -->
    <div v-if="showDeleteModal" class="modal">
      <div class="modal-content">
        <h3>Êtes-vous sûr de vouloir supprimer cet utilisateur?</h3>
        <button @click="deleteUser">Supprimer</button>
        <button @click="closeDeleteModal">Annuler</button>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { supabase } from '../supabase';
import CreationU from './CreationU.vue';

const utilisateurs = ref([]);
const showCreation = ref(false);
const showEditModal = ref(false);
const showDeleteModal = ref(false);
const editUtilisateur = ref({});
const userToDelete = ref(null);

const fetchUtilisateurs = async () => {
  const { data, error } = await supabase.from('utilisateur').select(`*, appartenir (groupe (nom, role))`);
  if (error) {
    console.error('Erreur lors de la récupération des utilisateurs:', error);
  } else {
    utilisateurs.value = data;
  }
};

const showCreationForm = () => {
  showCreation.value = true;
};

const hideCreationForm = () => {
  showCreation.value = false;
};

const editUser = (utilisateur) => {
  editUtilisateur.value = { ...utilisateur }; 
  showEditModal.value = true;
};

const closeEditModal = () => {
  showEditModal.value = false;
};

const updateUser = async () => {
  const { error } = await supabase
    .from('utilisateur')
    .update({
      pseudo: editUtilisateur.value.pseudo,
      email: editUtilisateur.value.email,
    })
    .eq('id', editUtilisateur.value.id);

  if (error) {
    console.error('Erreur lors de la mise à jour de l\'utilisateur:', error);
  } else {
    fetchUtilisateurs();
    closeEditModal();
  }
};

const confirmDeleteUser = (utilisateur) => {
  userToDelete.value = utilisateur; 
  showDeleteModal.value = true;
};

const deleteUser = async () => {
  const { error } = await supabase.from('utilisateur').delete().eq('id', userToDelete.value.id);

  if (error) {
    console.error('Erreur lors de la suppression de l\'utilisateur:', error);
  } else {
    fetchUtilisateurs();
    closeDeleteModal();
  }
};

const closeDeleteModal = () => {
  showDeleteModal.value = false;
};

onMounted(() => {
  fetchUtilisateurs();
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

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
  margin-left: 5px;

}

.modal button {
  background-color: #c59edb;
}

.modal button:hover {
  background-color: #b48ac6;
}

input {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}
</style>
