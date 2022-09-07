<script setup>

import { ref, computed } from 'vue'


// variables del sistema
const digitoCedula = 1
const empiezaMemoria = ref(digitoCedula * 10 + 50)
const empiezaKernel = ref(digitoCedula * 10 + 9)
const maxLength = empiezaMemoria.value - 1


const memoriaPrincipal = computed(() => {
  let memoria = new Array(empiezaMemoria.value)
  for (let i = 1; i <= empiezaKernel.value; i++) {
    memoria[i] = 0
  }
  return memoria
})


</script>

<template>
  <h1>CH maquina</h1>
  <div class="container">
    <div class="row">
      <div class="col">
        <p>Memoria</p>
        <input type="number" v-model="empiezaMemoria">
        <p>Kernel</p>
        <input type="number" v-model="empiezaKernel" :maxLength=max>
        <div class="row py-5">
          <div class="col">
            <table class="table table-bordered table-striped mb-0 table-dark table-hover" id="tablaMemoria">
              <thead>
                <tr>
                  <th scope="col">Posición</th>
                  <th scope="col">Valor</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(contenido, index) in memoriaPrincipal" :key="index">
                  <th scope="row">{{ index }}</th>
                  <td>{{ contenido }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="col">
            <table class="table table-bordered table-striped mb-0 table-dark table-hover" id="tablaMemoria">
              <thead>
                <tr>
                  <th scope="col">Posición</th>
                  <th scope="col">Valor</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(contenido, index) in memoriaPrincipal" :key="index">
                  <th scope="row">{{ index }}</th>
                  <td>{{ contenido }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="col">
        <p>col 2</p>
      </div>
      <div class="col">
        <p>Hola</p>
      </div>
      <div class="col">
        <div class="table-wrapper-scroll-y my-custom-scrollbar">
          <table class="table table-bordered table-striped mb-0 table-dark table-hover" id="tablaMemoria">
            <thead>
              <tr>
                <th scope="col">Posición</th>
                <th scope="col">Valor</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(contenido, index) in memoriaPrincipal" :key="index">
                <th scope="row">{{ index }}</th>
                <td>{{ contenido }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.my-custom-scrollbar {
  position: relative;
  height: 500px;
  overflow: auto;
}

.table-wrapper-scroll-y {
  display: block;
}

#tablaMemoria {
  font-size: 13px;
}
</style>
