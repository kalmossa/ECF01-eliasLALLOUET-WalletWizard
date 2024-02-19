<script setup>
import { ref, reactive } from 'vue';

let id = 0;

const operations = ref([]);

const operationDescription = ref('');
const operationMontant = ref('');

const supprimerOperation = (id) => {
  const index = operations.value.findIndex((operation) => operation.id === id);

  if (index !== -1) {
    operations.value.splice(index, 1);
  }
};

const ajouterOperation = () => {
  const montant = parseFloat(operationMontant.value);
  
  operations.value.push({
    id: operations.value.length + 1,
    description: operationDescription.value,
    montant: montant,
    date: new Date().toISOString().split('T')[0], // Date actuelle
  });

};

const calculerSomme = () => {
  return operations.value.reduce((total, operation) => total + operation.montant, 0);
};
</script>


<template>
<body>

  <div class="ensemble">
  <h2>Opérations de Crédit :</h2>
<form @submit.prevent="ajouterOperation">
  <label for="operation-description">Description :</label>
  <input type="text" id="operation-description" v-model="operationDescription" required>

  <label for="operation-montant">Montant : </label>
  <input type="number" id="operation-montant" v-model="operationMontant" required>€

  <button type="submit">Ajouter Opération</button>
</form>

<ul>
  <li v-for="operation in operations" :key="operation.id">
    {{ operation.description }} 
    <span v-if="operation.montant > 0">+{{ operation.montant }} € (Crédit)</span>
    <span v-else>{{ operation.montant }} € (Débit)</span>
    <button @click="supprimerOperation(operation.id)">Effacer</button>
  </li>
</ul>
<div>
  <h2>Somme Totale :</h2>
  <p>{{ calculerSomme() }} €</p>
</div>
</div>
 
</body>

</template>


<style scoped>

#app {
  width: 99dvw;
  height: 99dvh;
  margin: 0 auto;
  padding: 20px;
  background-color: #f0f5f9;
  border: 1px solid #a8c0d8;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  
}

h1, h2 {
  color: #2d83da;
}

form {
  margin-bottom: 30px;
}

label {
  display: block;
  margin-bottom: 8px;
  color: #34495e;
}

input {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  box-sizing: border-box;
  border: 1px solid #95a5a6;
  border-radius: 4px;
  font-size: 16px;
  background-color: #ecf0f1;
}

button {
  background-color: #27ae60;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  
}

button:hover {
  background-color: #219b56;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 15px;
  padding: 15px;
  background-color: #ffffff;
  border: 1px solid #bdc3c7;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li button {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 500ms ease,
        scale 500ms ease;
}

li button:hover {
  background-color: #c0392b;
  transition: background-color 500ms ease,
  scale 500ms ease;
}


</style>
