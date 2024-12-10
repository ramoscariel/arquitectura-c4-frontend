<template>
  <form @submit.prevent="submit">
    <label for="cedulaInput">Ingrese su cedula</label>
    <input v-model="cedula" type="number" id="cedulaInput" />
    <button type="submit">Consultar</button>
  </form>
  <div v-if="consulted">
    <p>Es Contribuyente: {{ esContribuyente? 'verdadero' : 'falso' }}</p>
    <p>Puntos Licencia: {{ puntos.points }}</p>
  </div> 
</template>

<script setup>
import axios from "axios";
import { ref } from 'vue';

const cedula = ref('');
const consulted = ref(false);

const esContribuyente = ref(null);
const puntos = ref(null);

const submit = async () => {
  consulted.value=true;
  esContribuyente.value = await consultarEsContribuyente(cedula.value + '001');
  puntos.value = await consultarPuntosLicencia(cedula.value);
}

const consultarEsContribuyente = async (numeroRuc) => {
  const url =
    `http://localhost:3000/proxy`
  try {
    const response = await axios.get(url, {
      params: { numeroRuc }
    });
    console.log('Response:', response.data);
    return response.data;
  } catch (error) {
    console.error('Error:', error.response?.data || error.message);
    throw error;
  }
};

const consultarPuntosLicencia = async (cedula) => {
  const url =
    `http://localhost:3001/points`;
  try {
    const response = await axios.get(url, {
      params: { cedula }
    });
    console.log('Response:', response.data);
    return response.data;
  } catch (error) {
    console.error('Error:', error.response?.data || error.message);
    throw error;
  }
}
</script>