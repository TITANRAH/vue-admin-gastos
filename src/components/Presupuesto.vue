<script setup>

import { ref } from 'vue';
import Alerta from './Alerta.vue'

const presupuesto = ref(0);
const error = ref('')

const emit = defineEmits(['definir-presupuesto'])

const definirPresupuesto = () => {
    if (presupuesto.value <= 0 || presupuesto.value === '') {
        error.value = 'Presupuesto no válido'
        setTimeout(() => {
            error.value = ''
        }, 2000)

        return
    }
        // emision al padre
    emit('definir-presupuesto', presupuesto.value)
  
}

</script>
<template>
    <form class="presupuesto" @submit.prevent="definirPresupuesto">
        <Alerta v-if="error">
            <!-- esto esta hecho con un slot que es como un children -->
            {{ error }}
        </Alerta>
        <div class="campo">
            <label for="">Definir Presupuesto</label>
            <input 
            type="number" 
            class="nuevo-presupuesto" 
            id="nuevo-presupuesto" 
            placeholder="Añade tu presupuesto"
            v-model.number="presupuesto" 
            min="0" 
            
            />
        </div>

        <input type="submit" value="Definir presupuesto">
    </form>
</template>


<style scoped>
.presupuesto {
    width: 100%;
    
}

.campo {
    display: grid;
    gap: 2rem;
}

.presupuesto label {
    font-size: 2.8rem;
    text-align: center;
    color: var(--azul);
}

.presupuesto input[type='number'] {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
    text-align: center;
}

.presupuesto input[type='submit'] {
    background-color: var(--azul);
    border: none;
    padding: 1rem;
    text-align: center;
    font-size: 2rem;
    margin-top: 2rem;
    color: var(--blanco);
    font-weight: 900;
    width: 100%;
    transition: background-color 300ms ease;
}

.presupuesto input[type='submit']:hover {
    background-color: #1048a4;
    cursor: pointer;
}
</style>