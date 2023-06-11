<script setup>

import ControlPresupuesto from './components/Control-presupuesto.vue';
import Presupuesto from './components/Presupuesto.vue';
import { ref, reactive } from 'vue';
import iconoNuevoGasto from './assets/img/nuevo-gasto.svg'
import Modal from './components/Modal.vue';


const presupuesto = ref(0);
const disponible = ref(0)
const gasto = reactive({
  nombre: '',
  cantidad: '',
  categoria: '',
  id: null,
  fecha: Date.now()
})

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad
  disponible.value = cantidad
}

// MODAL
const modal = reactive({
  mostrar: false,
  animar: false,
})

const mostrarModal = () => {
  modal.mostrar = true,

    setTimeout(() => {

      modal.animar = true
    }, 300)
}
const ocultarModal = () => {
  modal.mostrar = false,

    setTimeout(() => {

      modal.animar = false
    }, 300)
}

// FIN MODAL

</script>

<template >
  <div>
    <header class="contenedor-header">
      <h1>Planificador de gastos</h1>

      <div class="contenedor-header contenedor sombra">
        <!-- si el presupuesto es igual a cero que muestre presupuesto -->
        <Presupuesto v-if="presupuesto === 0" @definir-presupuesto="definirPresupuesto" />
        <!-- si es valido muestra control presupuesto -->
        <ControlPresupuesto :presupuesto="presupuesto" :disponible="disponible" v-else />
      </div>

    </header>

    <main v-if="presupuesto > 0">
      <div class="crear-gasto">
        <img :src="iconoNuevoGasto" alt="icono nuevo gasto" @click="mostrarModal">
      </div>

      <Modal 
        v-if="modal.mostrar" 
        @ocultar-modal="ocultarModal"
        :modal="modal"
        v-model:nombre="gasto.nombre"
        v-model:cantidad="gasto.cantidad"
        v-model:categoria="gasto.categoria"
        />
    </main>

  </div>
</template>

<!-- scope limita los estilos solo a este componente -->
<!-- si lo saco afectara a todos los estilos incliuso la de los compoenntes iomportados
por ejemplo si tengo un h1 en filtro y otroo aca afectara a los 2 -->
<style >
:root {
  --azul: #3b82f6;
  --blanco: #FFF;
  --gris-claro: #F5F5F5;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000;
}

html {
  font-size: 62.5%;
  box-sizing: border-box;
}

*,
*:before,
*:before {
  box-sizing: inherit;
}

body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--gris-claro);
}

h1 {
  font-size: 4rem;
}

h2 {
  font-size: 3rem;

}

header {
  background-color: var(--azul);
}

header h1 {
  padding: 3rem 0;
  margin: 0;
  color: var(--blanco);
  text-align: center;
}

.contenedor {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;

}

.contenedor-header {
  margin-top: -5rem;
  transform: translateY(5rem);

}

.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 5rem;

}

.crear-gasto {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
}

.crear-gasto img {
  width: 5rem;
  cursor: pointer;
}
</style>
