<script setup >
import { ref, computed } from 'vue';
import cerrarModal from '../assets/img/cerrar.svg'
import Alerta from './Alerta.vue';


// EN EL APP VUE ESTA ESTE HIJO MODAL 
// PASO EL OBJETO DESDE EL PADRE PARA PODER ASIGNARLE LAS CLASES 
// MIRAR Y ENTENDER

// ADEMAS LE ASIGNE VMODELS DESDE EL PADRE A ESTE COMPONENTE HIJO 
// AQUI LOS RECIBO EN LAS PROPS Y PARA ACTUALIZAR ESTOS CAMPOS DEBO HACERLO ASI 
// CON UPDATE: Y LUEGO EMITIR LOS EVENTOS asi 
// @input="$emit('update:nombre', $event.target.value)"

// seria la llamada en el input @input luego el emit luego lo que quiero emitir update:nombre y con que valor se 
// emitira y asignara al v model correspondiente
// y asi finalmente llenamos los campos del objeto padre gasto declarado en app.vue
// la forma de pasar un string a entero es asi para este caso 
// @input="$emit('update:nombre', +$event.target.value)" ponuendo un +

const emit = defineEmits(['ocultar-modal', 'guardar-gasto', "update:nombre", 'update:cantidad', 'update:categoria', 'eliminar-gasto'])
const error = ref('')
const props = defineProps({
    modal: {
        type: Object,
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    cantidad: {
        type: [String, Number],
        required: true
    },

    categoria: {
        type: String,
        required: true
    },
    disponible: {
        type: Number,
        required: true
    },
    id: {
        type: [String, null],
        required: true
    },

})

const old = props.cantidad;

const agregarGasto = () => {
    //   validar que no vengan campos vacios

    const { cantidad, categoria, nombre, disponible, id } = props

    if ([nombre, cantidad, categoria].includes('')) {
        error.value = 'Todos los campos son obligatorios'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    }
    if (cantidad <= 0) {
        error.value = 'Cantidad no válida'
        setTimeout(() => {
            error.value = ''
        }, 3000)
        return
    }

    if (id) {
        if (cantidad > old + disponible) {

            // si la cantidad a gastar actual es mayor a la cantidad antigua mas lo que dispones
            error.value = 'Has excedido el presupuesto'
            setTimeout(() => {
                error.value = ''
            }, 3000)
            return
        }
    } else {
        // si lo que voy a gastar es mayor a lo disponible 
        if (cantidad > disponible) {
            error.value = 'Has excedido el presupuesto'
            setTimeout(() => {
                error.value = ''
            }, 3000)
            return
        }
    }



    emit('guardar-gasto')
}

const idEditing = computed(() => {

    return props.id
})
</script>
<template>
    <div class="modal">
        <div class="cerrar-modal">
            <img :src="cerrarModal" alt="cerrar modal" @click="$emit('ocultar-modal')">
        </div>

        <div class="contenedor contenedor-formulario" :class="[modal.animar ? 'animar' : 'cerrar']">
            <form class="nuevo-gasto" @submit.prevent="agregarGasto">
                <legend>{{ idEditing ? 'Editar gasto' : 'Guardar gasto' }}</legend>
                <Alerta v-if="error">{{ error }}</Alerta>
                <div class="campo">
                    <label for="nombre">Nombre Gasto: </label>
                    <input type="text" id="nombre" placeholder="Añade el Nombre del Gasto" :value="nombre"
                        @input="$emit('update:nombre', $event.target.value)">
                </div>
                <div class="campo">
                    <label for="cantidad">Cantidad: </label>
                    <input type="number" id="cantidad" placeholder="Añade la cantidad del gasto, ejm 300" :value="cantidad"
                        @input="$emit('update:cantidad', +$event.target.value)">
                </div>
                <div class="campo">
                    <label for="cantidad">Categoría: </label>
                    <select id="categoria" :value="categoria" @input="$emit('update:categoria', $event.target.value)">
                        <option value="">-- Seleccione --</option>
                        <option value="ahorro">Ahorro</option>
                        <option value="comida">Comida</option>
                        <option value="casa">Casa</option>
                        <option value="gastos">Gastos Varios</option>
                        <option value="ocio">Ocio</option>
                        <option value="salud">Salud</option>
                        <option value="suscripciones">Suscripcion</option>
                    </select>
                </div>

                <input type="submit" :value="id ? 'Guardar Cambios' : 'Añadir Gasto'">
            </form>

            <button v-if="idEditing" type="button" class="btn-eliminar" @click="$emit('eliminar-gasto')">
                Eliminar Gasto
            </button>
        </div>
    </div>
</template>


<style scoped>
.btn-eliminar {
    padding: 1rem;
    width: 100%;
    background-color: #ef4444;
    font-weight: 700;
    font-size: 1.2rem;
    color: var(--blanco);
    margin-top: 10rem;
    border: none;
    cursor: pointer;
}

.modal {
    position: absolute;
    background-color: rgb(0 0 0 / 0.9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}

.cerrar-modal {
    position: absolute;
    right: 3rem;
    top: 3rem;
}

.cerrar-modal img {
    width: 3rem;
    cursor: pointer;
}

.contenedor-formulario {
    transition-property: all;
    transition-duration: 300ms;
    transition-timing-function: ease-in;
    opacity: 0;
}

.contenedor-formulario.animar {

    opacity: 1;
}

.contenedor-formulario.cerrar {

    opacity: 0;
}

/* FORM */
.nuevo-gasto {
    margin: 10rem auto 0 auto;
    display: grid;
    gap: 2rem;
}

.nuevo-gasto legend {
    text-align: center;
    color: var(--blanco);
    font-size: 3rem;
    font-weight: 700;
}

.campo {
    display: grid;
    gap: 2rem;
}


.nuevo-gasto input,
.nuevo-gasto select {
    background-color: var(--gris-claro);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    font-size: 2.2rem;
}

.nuevo-gasto label {
    color: var(--blanco);
    font-size: 3rem;
}

.nuevo-gasto input[type="submit"] {
    background-color: var(--azul);
    color: var(--blanco);
    font-weight: 700;
    cursor: pointer;
}

/* FIN FORM */
</style>