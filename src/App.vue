<script setup>

  import { ref,computed } from 'vue'

  // variables del sistema
  const digitoCedula = 1
  const empiezaMemoria = ref(digitoCedula *10 + 50)
  const empiezaKernel = ref(10* digitoCedula + 9)

  const memoriaPrincipal = computed(() => {
    const array = []
    for (let i = empiezaMemoria.value; i < empiezaMemoria.value + 50; i++) {
      array.push(i)
    }
    return array
  })


</script>

<template>
  <h1>CH maquina</h1>
  <div class="container">
    <div class="row">
      <div class="col">
        <p>Memoria</p>
        <input type="number" v-model = "empiezaMemoria">
        <p>Kernel</p>
        <input type="number" v-model = "empiezaKernel">
      </div>
      <div class="col"></div>
      <div class="col">
        <div class="table-wrapper-scroll-y my-custom-scrollbar">
          <table class="table table-bordered table-striped mb-0 table-dark table-hover">
            <thead>
              <tr>
                <th scope="col">Posicion</th>
                <th scope="col">Valor</th>
              </tr>
            </thead>
            <tbody>

              <tr v-for="(valor, index) in memoriaPrincipalTabla" :key="index">
                <td>{{index}}</td>
                <td>{{valor}}</td>
              </tr>

              <tr v-for="(value, index) in memoriaPrincipal" :key="index">
                <th scope="row">{{ index }}</th>
                <td>{{ value }}</td>
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
</style>
