<template>
  <div class="collaborateur-container">
    <header class="collab-header">
      <h1>Espace Collaborateur</h1>
      <div class="user-info">
        <span>{{ username }}</span>
        <button @click="logout">Déconnexion</button>
      </div>
    </header>

    <nav class="collab-nav">
      <ul>
        <li @click="showSection('evaluation')">Évaluations</li>
        <li @click="showSection('note')">Notes</li>
      </ul>
    </nav>

    <div class="collab-content">
      <div v-if="currentSection === 'evaluation'">
        <h2>Évaluations</h2>
        <!-- Contenu relatif aux évaluations -->
      </div>
      <div v-if="currentSection === 'note'">
        <h2>Notes</h2>
        <!-- Contenu relatif aux notes -->
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  username: {
    type: String,
    required: true
  }
});

const emit = defineEmits();
const currentSection = ref('evaluation');

const showSection = (section) => {
  currentSection.value = section;
};

const logout = () => {
  emit('logout');
};
</script>

<style scoped>

.collaborateur-container {
  padding: 20px;
}

.collab-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.user-info {
  display: flex;
  align-items: center;
}

.user-info span {
  margin-right: 20px;
}

.collab-nav {
  margin-top: 20px;
}

.collab-nav ul {
  list-style-type: none;
  padding: 0;
}

.collab-nav li {
  display: inline;
  margin-right: 20px;
  cursor: pointer;
}

.collab-content {
  margin-top: 30px;
}
</style>
