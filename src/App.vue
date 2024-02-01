<script setup lang="ts">
import { ref, onMounted } from "vue";
import type { Ref } from "vue";

const Nombre: Ref<string> = ref("");
const Apellido: Ref<string> = ref("");
const Edad: Ref<number> = ref(0);
const Genero: Ref<string> = ref("");
const GeneroPersonalizado: Ref<string> = ref("");
const errors: Ref<{[key: string]:string}> = ref({});

const validarForm = () => {
  errors.value = {};

  if (Nombre.value.trim() === "") {
    errors.value.Nombre="El campo Nombre no puede estar vacío";
  } else if (!/^[A-Za-zÀ-ÖØ-öø-ÿ\s]+$/.test(Nombre.value.trim())) {
    errors.value.Nombre = "El nombre solo puede contener letras y espacios";
  }

  if (Apellido.value.trim() === "") {
    errors.value.Apellido="El campo Apellido no puede estar vacío";
  } else if (!/^[A-Za-zÀ-ÖØ-öø-ÿ\s]+$/.test(Apellido.value.trim())) {
    errors.value.Apellido = "El apellido solo puede contener letras y espacios";
  }

  // Validar el Nombre
  if (Nombre.value.length < 5 || Nombre.value.length > 18) {
    errors.value.Nombre="El nombre debe tener entre 5 y 18 caracteres";
  }

  // Validar el apellido
  if (Apellido.value === Nombre.value) {
    errors.value.Apellido="El Apellido no debe ser igual al nombre";
  }

  // Validar la Edad
  if (isNaN(Edad.value) || Edad.value <= 0 || Edad.value >= 60) {
    errors.value.Edad="la edad debe ser un número mayor a 0 y menor a 60";
  }

  // Validar el Genero
  if (!["Masculino", "Femenino", "Otro"].includes(Genero.value)) {
    errors.value.Genero="Genero inválido";
  } else if (Genero.value === "Otro" && GeneroPersonalizado.value === "") {
    errors.value.GeneroPersonalizado="Ingresa el genero";
  }
};

onMounted(() => {
  validarForm();
});

</script>

<template>
  <main class="form">
    <div>
      <label>Nombre:</label>
      <input @input="validarForm()" v-model="Nombre" type="text" id="nombre" pattern="[A-Za-zÀ-ÖØ-öø-ÿ\s]+" required>
      <span v-if="errors.Nombre" class="error">{{ errors.Nombre }}</span>
    </div>

    <div>
      <label>Apellido:</label>
      <input @input="validarForm()" v-model="Apellido" type="text">
      <span v-if="errors.Apellido" class="error">{{ errors.Apellido }}</span>
    </div>

    <div>
      <label>Edad:</label>
      <input @input="validarForm()" v-model.number="Edad" type="number">
      <span v-if="errors.Edad" class="error">{{ errors.Edad }}</span>
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
        placeholder="Ingresa el nuevo género"
      />
      <span v-if="errors.Genero || errors.GeneroPersonalizado" class="error">{{ errors.Genero || errors.GeneroPersonalizado }}</span>
    </div>

    <h1 class="msj">Hola {{Nombre}} {{ Apellido }}, tienes {{ Edad }} años y tu género es {{ Genero === 'Otro' ? GeneroPersonalizado : Genero }}</h1>

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
    margin: 15px 0 0 0;
  }

  input,
  select {
    width: 100%;  
    padding: 8px;
    margin-bottom: 15px;
    box-sizing: border-box;
    border-radius: 10px;
  }

  h1 {
    margin-top: 20px;
    font-size: 1.5em;
  }

  .form{
    background-color:rgb(21, 21, 76);
    border-radius: 10px;
  }
  .error {
    color: white;
    background-color: red;
    border-radius: 5px;
    padding: 5px;
  }

  .msj {
    color: white;
    border: 1px solid white;
    border-radius: 10px;
    padding: 7px;
  }
</style>
