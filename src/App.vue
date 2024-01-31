<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";

const Nombre: Ref<string> = ref("");
const Apellido: Ref<string> = ref("");
const Edad: Ref<number> = ref(0);
const Genero: Ref<string> = ref("");
const GeneroPersonalizado: Ref<string> = ref("");
const errors: Ref<string[]> = ref([]);

const greetingMessage: Ref<string> = ref("");

const validarForm = () => {
  errors.value = [];

  // Validar el Nombre
  if (Nombre.value.length < 5 || Nombre.value.length > 18) {
    errors.value.push("El nombre debe tener entre 5 y 18 caracteres");
  }

  // Validar el apellido
  if (Apellido.value === Nombre.value) {
    errors.value.push("El Apellido no debe ser igual al nombre");
  }

  // Validar la Edad
  if (Edad.value <= 0 || Edad.value >= 60) {
    errors.value.push("la edad debe ser mayor a 0 y menor a 60");
  }

  // Validar el Genero
  if (!["Masculino", "Femenino", "Otro"].includes(Genero.value)) {
    errors.value.push("Genero inválido");
  } else if (Genero.value === "Otro" && GeneroPersonalizado.value === "") {
    errors.value.push("Ingresa el genero");
  }
};


</script>

<template>
  <main>
    <div>
      <label>Nombre:</label>
      <input @input="validarForm()" v-model="Nombre" type="text">
    </div>

    <div>
      <label>Apellido:</label>
      <input @input="validarForm()" v-model="Apellido" type="text">
    </div>

    <div>
      <label>Edad:</label>
      <input @input="validarForm()" v-model.number="Edad" type="number">
    </div>

    <div>
      <label>Genero:</label>
      <select @change="validarForm()" v-model="Genero">
        <option value="Masculino">Masculino</option>
        <option value="Femenino">Femenino</option>
        <option value="Otro">Otro</option>
      </select>

      <!-- Input Otro -->
      <input
        v-if="Genero === 'Otro'"
        @input="validarForm()"
        v-model="GeneroPersonalizado"
        type="text"
        placeholder="Ingresa el género"
      />
    </div>

    <h1 :class="{'msj':Nombre}">Hola {{Nombre}} {{ Apellido }}, tienes {{ Edad }} años y tu género es {{ Genero === 'Otro' ? GeneroPersonalizado : Genero }}.</h1>

    <h3>Errores:</h3>
    <span v-for="(err, index) in errors" :key="index" class="error">
      {{ err }}
      <br>
    </span>
  </main>
</template>

<style scoped>

main {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
  }

  label {
    display: block;
    margin-bottom: 5px;
  }

  input,
  select {
    width: 100%;
    padding: 8px;
    margin-bottom: 15px;
    box-sizing: border-box;
  }

  h1 {
    margin-top: 20px;
    font-size: 1.5em;
  }
  .error {
    color: white;
    background-color: red;
    border: 1px solid white;
  }

  .msj {
    color: white;
    background-color: rgb(175, 0, 188);
    border: 1px solid white;
  }

</style>
