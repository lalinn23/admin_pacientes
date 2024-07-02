<script setup>
import { reactive } from "vue";
import Alerta from "./Alerta.vue";

//agregamos las alertas
const alerta = reactive({
    tipo: '',
    mensaje: ''
})


const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', ,'update:alta', 
'update:sintomas', 'guardar-paciente'])

const props = defineProps({
    nombre: {
        type: String,
        required: true
    },
    propietario: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    alta: {
        type: String,
        required: true
    },
    sintomas: {
        type: String,
        required: true
    },
})


//validamos
const validar = () => {
    console.log(Object.values(props));
    if(Object.values(props).includes('')){
       alerta.mensaje = 'Todos los campos son obligatorios';
       alerta.tipo = 'error'
        return

    }
    //para que se detone la funcion guardar paciente
    emit('guardar-paciente')
    //para cambiar el color de la alerta si es success
    alerta.mensaje = 'Paciente Almacenado Corectamente'
    alerta.tipo = 'exito'

    //tiempo de alerta
    setTimeout(() => {
      Object.assign(alerta, {
        tipo: '',
        mensaje: ''
      })
    }, 3000)
}


</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Paciente</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo-600 font-bold">Administralos</span>
    </p>
    <!-- solo se detonara cuando aya un mensaje de alerta -->
    <Alerta 
        v-if="alerta.mensaje"
        :alerta="alerta"
    />

    <form class="bg-white shadow-2xl rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          nombre del paciente
        </label>

        <input
          id="mascota"
          type="text"
          placeholder="Nombre del paciente"
          class="border-2 w-full p-2 mt-2 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
          
        />
      </div>

      <div class="mb-5">
        <label
          for="propietario"
          class="block text-gray-700 uppercase font-bold"
        >
          Apellidos
        </label>

        <input
          id="propietario"
          type="text"
          placeholder="Apellidos"
          class="border-2 w-full p-2 mt-2 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
          
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          email
        </label>

        <input
          id="email"
          type="text"
          placeholder="example@example.com.mx"
          class="border-2 w-full p-2 mt-2 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
          
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          Fecha de ingreso
        </label>

        <input
          id="alta"
          type="date"
          class="border-2 w-full p-2 mt-2 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"

          
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Sintomas
        </label>

        <textarea
          id="sintomas"
          placeholder="Describe los sintomas del paciente"
          class="border-2 w-full p-2 mt-2 rounded-md h-40"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"

          
        />
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-green-500 cursor-pointer transition-colors"
        value="Registrar Paciente"
      />
    </form>
  </div>
</template>
