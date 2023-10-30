<script setup>
// Importando la funcionalidad de propiedades computadas 
import { ref, computed } from "vue";
const header = ref('App Lista de compras');
const items = ref([
 {id: 1, label: '10 bolillos', purchased: true, highPriority: true},
 {id: 2, label: '1 lata de frijoles', purchased: false, highPriority: false},
 {id: 3, label: '2 lata de atún', purchased: true, highPriority: true}
]);
// Funcion que alterna el estado de comprado de un item
const togglePurchased = (item) => {
  //Invertir la propiedad "purchased"
item.purchased = !item.purchased;
}
// Agregando metodo para agregar nuevo articulo en la lista
const saveItem = () => {
  items.value.push({id: items.value.length + 1, label: newItem.value})
  // Limpiando el contenido de nexItem
  newItem.value = "";
};
const newItem = ref('');
const newItemHighPriority = ref(false);
const editing = ref(false);
const doEdit = (edit) => {
  // Altero la variable "editing"
  editing.value = edit;
  // Limpio el imput de texto 
  newItem.value = "";
};
// A continuación, creamos una referencia constante llamada characterCount y le pasamos una instancia creada con computed(), que utiliza una función para formar la propiedad computada
// Creando una propiedad computada
const characterCount = computed(()=>{
  // Toda propiedad computada debe regresar un valor
  return newItem.value.length;
});
// Como segundo ejemplo de uso de las propiedades computadas
// Creando propiedad computada que invierte items de la lista
const reversedItems = computed(() => {
  // La forma correcta de emplearlo es de la siguiente manera
  return [...items.value].reverse();
});
</script>

<template>
  <div class="header">
   <h1> <i class="material-icons shopping-cart-icon">local_mall</i> {{ header }}</h1>
  <button v-if="!editing" @click="doEdit(true)" class="btn btn-primary">Agregar Articulo</button>
  <button v-else @click="doEdit(false)" class="btn">Cancelar</button>
  </div>
  <!-- <a v-bind:href="newItem">
    <i class="material-icons shopping-cart-icon">link</i>
  </a> -->
  <form v-if="editing" v-on:submit.prevent="saveItem" class="add-item form">
    <!-- Input de Nuevo Articulo -->
    <input v-model.trim="newItem" type="text" placeholder="Ingresar nuevo articulo" required>
    <!-- Check Boxes -->
    <label>
      <input v-model="newItemHighPriority" 
      type="checkbox">
      Alta Prioridad
    </label>
    {{ newItemHighPriority ? "🔥" : "🧊" }}
    <!-- Boton de UI -->
    <button class="btn btn-primary">Guardar Articulo</button>
   <!--Ahora procederemos a integrar a la plantilla la información de la propiedad computada.-->
    <!-- Contador -->
  <p class="counter">
    {{ characterCount }} / 200
  </p>
  </form>
  <ul>
    <!--Por último para usar esto en nuestra plantilla, reemplazaremos la variable que se usa para generar la lista en la interfaz por la propiedad computada.-->
    <li 
    v-for="({ id, label, purchased, highPriority }, index) in reversedItems" 
    v-bind:key="id"
    :class="{ strikeout : purchased, priority: highPriority}"
    @click="togglePurchased(reversedItems[index])"
    >
      🔹{{ label }}
    </li>
  </ul>
  <p v-if="items.length == 0">🥀Lista de compras vacia🥀</p>
  <p v-else>🔥 Ingrese mas items🔥</p>
</template>

<style scoped>
.shopping-cart-icon {
  font-size: 2rem; /* Adjust the font-size value as per your desired size */
}
</style>
