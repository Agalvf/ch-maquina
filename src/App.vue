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

const acumulador = ref(0);
const nombresValores = ref({})

const diccionario = ["nueva",
  "almacene", "cargue", "lea", "sume", "reste", "multiplique", "divida",
  "potencia", "modulo", "concatene", "elimine", "extraiga",
  "Y", "O", "NO", "muestre", "imprima", "retorne", "vaya", "vayasi", "etiqueta"]

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
          if (diccionario.includes(element.split(" ")[0])) {
            const expreregular = /(\s{2,})/g;
            let remplazar = element.replace(expreregular, " ");
            instrucciones.value.push(remplazar)
            if (remplazar.split(" ")[0] == "nueva") {
              variables.value.push(remplazar.split(" ")[1])
            }
            else if (remplazar.split(" ")[0] == "etiqueta") {
              etiquetas.value.push(element.split(" ")[1])
            }
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

const ejecutar = () => {
  for (let i = 0; i < instrucciones.value.length; i++) {
    switch (instrucciones.value[i].split(" ")[0].toLowerCase()) {
      case "cargue":
        acumulador.value = nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "almacene":
        nombresValores.value[instrucciones.value[i].split(" ")[1]] = acumulador.value
        break;
      case "nueva":
        if (instrucciones.value[i].split(" ")[2].toLowerCase() == "c") {
          nombresValores.value[instrucciones.value[i].split(" ")[1]] = "" + instrucciones.value[i].split(" ")[3]
        }
        else if (instrucciones.value[i].split(" ")[2].toLowerCase() == "i") {
          nombresValores.value[instrucciones.value[i].split(" ")[1]] = Number(instrucciones.value[i].split(" ")[3])
        }
        else if (instrucciones.value[i].split(" ")[2].toLowerCase() == "r") {
          nombresValores.value[instrucciones.value[i].split(" ")[1]] = parseFloat(instrucciones.value[i].split(" ")[3])
        }
        else {
          nombresValores.value[instrucciones.value[i].split(" ")[1]] = instrucciones.value[i].split(" ")[3] == 1 ? true : false
        }
        break;
      case "lea":
        nombresValores.value[instrucciones.value[i].split(" ")[1]] = prompt("Ingrese un valor")
        break;
      case "sume":
        acumulador.value += nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "reste":
        acumulador.value -= nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "multiplique":
        acumulador.value *= nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "divida":
        acumulador.value /= nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "potencia":
        acumulador.value = Math.pow(acumulador.value, nombresValores.value[instrucciones.value[i].split(" ")[1]])
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "modulo":
        acumulador.value %= nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "concatene":
        acumulador.value += nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "elimine":
        acumulador.value = acumulador.value.slice(0, -1)
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "extraiga":
        acumulador.value = acumulador.value.slice(0, nombresValores.value[instrucciones.value[i].split(" ")[1]])
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "y":
        acumulador.value = acumulador.value && nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "o":
        acumulador.value = acumulador.value || nombresValores.value[instrucciones.value[i].split(" ")[1]]
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "no":
        acumulador.value = !acumulador.value
        memoriaPrincipal.value[0] = acumulador.value
        break;
      case "muestre":
        if(instrucciones.value[i].split(" ")[1].toLowerCase() == "acumulador"){
          alert(acumulador.value)
        }
        else{
          alert(nombresValores.value[instrucciones.value[i].split(" ")[1]])
        }
      break;
      case "imprima":
      break;
      case "vaya":
        i = instrucciones.value.findIndex(instrucciones.value[i].split(" ")[1])
        break;
      case "vayasi":
        if(acumulador.value > 0){
          const a = instrucciones.value.find(
            function(element) {
               if(element.split(" ")[0] === "etiqueta" && element.split(" ")[1] === instrucciones.value[i].split(" ")[1]){
                 return element.split(" ")[2] 
               }
            }
          )
          i = parseInt(a.split(" ")[2]) -2
        }
        else if(acumulador.value < 0){
          const a = instrucciones.value.find(
            function(element) {
               if(element.split(" ")[0] === "etiqueta" && element.split(" ")[1] === instrucciones.value[i].split(" ")[2]){
                 return element.split(" ")[2] 
               }
            }
          )
          i = parseInt(a.split(" ")[2]) -2
        }
        else
          break;
    }
  }
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
            <a class="nav-link" href="#" @click="ejecutar">Ejecutar</a>
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
          <input type="number" v-model="empiezaKernel">
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
