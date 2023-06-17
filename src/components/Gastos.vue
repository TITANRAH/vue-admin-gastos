<script setup>

import {formatearCantidad, formatearFecha} from '../helpers/index'
import IconoAhorro from '../assets/img/icono_ahorro.svg'
    import IconoCasa from '../assets/img/icono_casa.svg'
    import IconoComida from '../assets/img/icono_comida.svg'
    import IconoGastos from '../assets/img/icono_gastos.svg'
    import IconoOcio from '../assets/img/icono_ocio.svg'
    import IconoSalud from '../assets/img/icono_salud.svg'
    import IconoSuscripciones from '../assets/img/icono_suscripciones.svg'

    // en el select tengo una serie de nombres por ejemplo comida 
    // al seleccionar comida, me creo un diccionario con iconos segun gasto 
    // al pulsar en el select por ejemplpo selecciono comida 
    // y digo abajo que la imagen a mostrar sera el diccionario en el nombre de la seleccion 
    // entonces si selecciono comida, y digo que la iamagen es diccionario[gasto.categoria]
    // eso equivale a decir la imagen es diccionarioIconos en la posicion que diga comida y 
    // pintara el icono de comida
    const diccionarioIconos = {
        ahorro : IconoAhorro,
        comida : IconoComida,
        casa : IconoCasa,
        gastos : IconoGastos,
        ocio : IconoOcio,
        salud : IconoSalud,
        suscripciones : IconoSuscripciones
    }

    const props = defineProps({
    gasto: {
        type: Object,
        required: true
    }

})

</script>
<template>
    <div class="gasto sombra">
       <div class="contenido">
        <img 
            :src="diccionarioIconos[gasto.categoria]"
            alt="Icono gasto"
            class="icono">
        <div class="detalles">
            <p class="categoria">{{ gasto.categoria }}</p>
            <p class="nombre">{{ gasto.nombre }}</p>
            <p class="fecha">{{formatearFecha(gasto.fecha)  }}</p>
        </div>
       </div>

       <p class="cantidad">
        {{ formatearCantidad(gasto.cantidad)  }}
       </p>
    </div>
</template>


<style scoped>

gasto {

}
.contenido {

}


</style>