<template>
  <main class="app-container">
    <div class="input-container">
      <input @input="validateName" v-model="name" type="text" placeholder="Nombre" class="input-field">
    </div>
    <div class="input-container">
      <input @input="validateLastName" v-model="lastName" type="text" placeholder="Apellido" class="input-field">
    </div>
    <div class="input-container">
      <input @input="validateAge" v-model="age" type="text" id="age" class="input-field" placeholder="Ingresa la Edad">
    </div>
    <div class="input-container">
      <select v-model="gender" id="gender" class="input-field">
        <option value="male">Masculino</option>
        <option value="female">Femenino</option>
      </select>
    </div>

    <h1 v-if="isNameValid" :style="{ fontSize: '2em', color: 'green', padding: '1em', border: '1px solid #ccc' }">{{ name }} {{ lastName }} - ¡Nombre válido!</h1>
    <h2 v-if="isAgeValid" :class="{ error: age < 0 || age > 60, fontSize: '1.5em', color: 'green' }">{{ age }} - ¡Edad válida!</h2>

    <h3 style="font-size: 1.2em;">Errores:</h3>
    <span v-for="(err, index) in errors" :key="index" :class="{ 'error-message': err.name || err.lastName || err.age }" style="font-size: 1.2em;">
      {{ err.name }} {{ err.lastName }} {{ err.age }}
    </span>

    <button @click="showData" v-if="isNameValid && isAgeValid" class="submit-button">Mostrar Datos</button>
    
    <div v-if="showUserData" class="user-data">
      <h3>Datos Ingresados:</h3>
      <p><strong>Nombre:</strong> {{ name }} {{ lastName }}</p>
      <p><strong>Edad:</strong> {{ age }}</p>
      <p><strong>Sexo:</strong> {{ gender === 'male' ? 'Masculino' : (gender === 'female' ? 'Femenino' : 'Otro') }}</p>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

interface ValidationError {
  name: string;
  lastName: string;
  age: string;
}

const name = ref('Carlos');
const lastName = ref('Lair');
const age = ref('');
const gender = ref('male');
const errors = ref<ValidationError[]>([]);

const showUserData = ref(false);

const validateName = () => {
  errors.value = [];
  if (name.value.length < 5) {
    errors.value.push({ name: 'Nombre invalido', lastName: '', age: '' });
  }

  if (name.value === lastName.value) {
    errors.value.push({ name: 'Nombre y Apellido no pueden ser iguales', lastName: '', age: '' });
  }
};

const validateLastName = () => {
  errors.value = [];
  if (lastName.value.length < 5) {
    errors.value.push({ name: '', lastName: 'Apellido invalido', age: '' });
  }

  if (name.value === lastName.value) {
    errors.value.push({ name: 'Nombre y Apellido no pueden ser iguales', lastName: '', age: '' });
  }
};

const validateAge = () => {
  errors.value = [];
  const ageNumber = parseInt(age.value, 10);
  if (isNaN(ageNumber) || ageNumber < 0 || ageNumber > 60) {
    errors.value.push({ name: '', lastName: '', age: 'Edad debe estar entre 0 y 60 años' });
  }
};

const isNameValid = computed(() => errors.value.every(err => !err.name));
const isAgeValid = computed(() => errors.value.every(err => !err.age));

const showData = () => {
  showUserData.value = true;
};
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  margin: 0;
}

.input-container {
  margin-bottom: 1em;
}

.input-field {
  font-size: 1.5em;
  padding: 0.5em;
}

.error {
  font-size: 1.5em;
  color: white;
  background-color: red;
  border: 1px solid white;
}

.error-message {
  color: red;
  font-weight: bold;
  margin-top: 0.5em;
}

.submit-button {
  font-size: 1.5em;
  padding: 0.5em 1em;
  margin-top: 1em;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
}

.user-data {
  margin-top: 2em;
  border: 1px solid #ccc;
  padding: 1em;
}
</style>
