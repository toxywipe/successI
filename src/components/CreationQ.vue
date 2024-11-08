<template>
    <div>
      <h2>Créer un nouveau questionnaire</h2>
      <form @submit.prevent="createQuestionnaire">
        <label>Nom du QCM:
          <input v-model="qcm.nom" type="text" required />
        </label>
        <label>Catégorie:
          <input v-model="qcm.categorie" type="text" required />
        </label>
        <label>Groupe:
          <input v-model="qcm.groupe" type="text" required />
        </label>
        <button type="submit">Créer</button>
        <button type="button" @click="cancel">Annuler</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { supabase } from '../supabase';
  
  const emit = defineEmits(['refresh', 'cancel']);
  
  const qcm = ref({
    nom: '',
    categorie: '',
    groupe: ''
  });
  
  const createQuestionnaire = async () => {
    const { error } = await supabase.from('questionnaires').insert([qcm.value]);
  
    if (error) {
      console.error('Erreur lors de la création du questionnaire:', error);
    } else {
      console.log('Questionnaire créé avec succès');
      qcm.value = { nom: '', categorie: '', groupe: '' };
      emit('refresh');
    }
  };
  
  const cancel = () => {
    emit('cancel');
  };
  </script>
  
  <style scoped>
  form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-top: 20px;
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
  </style>
  