<script setup>

import { ref, computed } from 'vue'


// variables del sistema
const digitoCedula = 1
const empiezaMemoria = ref(digitoCedula * 10 + 50)
const empiezaKernel = ref(digitoCedula * 10 + 9)

const instrucciones = ref(new Array(10).fill("none"))
const memoriaPrincipal = ref(new Array(empiezaMemoria.value));
const variables = ref(new Array(10).fill("none"));
const etiquetas = ref(new Array(10).fill("none"));

/* const memoriaPrincipal = computed(() => {
  let memoria = new Array(empiezaMemoria.value)
  for (let i = 1; i <= empiezaKernel.value; i++) {
    memoria[i] = 0
  }
  return memoria

}) */


const cargar = () => {
  const input = document.createElement("input");
  input.type = "file";
  input.onchange = (e) => {
    const file = e.target?.files[0];
    const reader = new FileReader();
    reader.onload = (e) => {
      const text = e.target.result;
      if (text) {
        const lines = text.toString().split("\n");
        instrucciones.value = []
        variables.value = []
        etiquetas.value = []
        lines.forEach(element => {
          element = element.trim()
          if (element.includes("nueva")) {
            let lineaSinEspacios = element.split(" ")
            if (lineaSinEspacios[1] == "") {
              variables.value.push(lineaSinEspacios[2])
            }
            else {
              variables.value.push(lineaSinEspacios[1])
            }
          }
          if (element.includes("etiqueta")) {
            let lineaSinEspacios = element.split(" ")
            etiquetas.value.push(lineaSinEspacios[1])
          }
          if (element.includes("//") == false && element != "") {
            instrucciones.value.push(element)
          }
        });
        for (let i = 1; i <= empiezaKernel.value; i++) {
          memoriaPrincipal.value[i] = "--** CH MAQUINA **--"
        }
        for (let i = 0; i < instrucciones.value.length; i++) {
          memoriaPrincipal.value[i + empiezaKernel.value + 1] = instrucciones.value[i]
        }

      }
    };
    reader.readAsText(file);
  };
  input.click();
}
</script>

<template>
  <div class="">
    <nav class="navbar navbar-expand-lg bg-light">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <a class="nav-link" aria-current="page" href="#" @click="cargar">Home</a>
            <a class="nav-link" href="#">Features</a>
            <a class="nav-link" href="#">Pricing</a>
          </div>
        </div>
      </div>
    </nav>
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
                    <th scope="col">Instrucciones</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(contenido, index) in instrucciones" :key="index">
                    <td>{{ contenido }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="col">
              <table class="table table-bordered table-striped mb-0 table-dark table-hover" id="tablaMemoria">
                <thead>
                  <tr>
                    <th scope="col">Variables</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(contenido, index) in variables" :key="index">
                    <td>{{ contenido }}</td>
                  </tr>
                </tbody>
              </table>
              <table class="table table-bordered table-striped mb-0 table-dark table-hover my-2" id="tablaMemoria">
                <thead>
                  <tr>
                    <th scope="col">Etiquetas</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(contenido, index) in etiquetas" :key="index">
                    <td>{{ contenido }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
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
