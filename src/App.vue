<script setup>
  import {ref, reactive, watch, onUnmounted } from 'vue'
  import { uid } from 'uid';
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue';
  
  const pacientes = ref([])

    //* leemos los datos
  const paciente = reactive({
      id: null,
      nombre: '',
      propietario: '',
      email:'',
      alta:'',
      sintomas: '',
  })
    watch(pacientes, () => {
      guardarLocalStorage()
    }, {
      deep: true
    })

  //guardammos la informacion en localStorage
  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  onUnmounted(() => {
    const pacientesStorage = localStorage.getItem('pacientes')
    if (pacientesStorage) {
      pacientes.value = JSON.parse(pacientesStorage)
    }
  })



  //registramos un nuevo evento para actualizar y que se llene el formulario
  const actualizarPaciente = (id) => {
    const pacienteEditar = pacientes.value.filter( paciente => paciente.id === id)[0]
    Object.assign(paciente ,pacienteEditar)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter( paciente => paciente.id != id)
  }



  //guardar pasieente
  const guardarPaciente = () => {
    if(paciente.id){
      const { id } = paciente
      const i = pacientes.value.findIndex((pacienteState) => pacienteState.id === id)
      pacientes.value[i] = {...paciente}
    }else{
      pacientes.value.push({
        ...paciente,
        id: uid() //le pasamos el id
      })
    }


    // Reiniciar el objeto
    paciente.nombre = ''
    paciente.propietario = ''
    paciente.email = ''
    paciente.alta = ''
    paciente.sintomas = ''
  }

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
        <Formulario  
          v-model:nombre="paciente.nombre"
          v-model:propietario="paciente.propietario"
          v-model:email="paciente.email"
          v-model:alta="paciente.alta"
          v-model:sintomas="paciente.sintomas"
          @guardar-paciente = "guardarPaciente"
          :id="paciente.id"

        />
        
        <div class="md:w-1/2 md:h-screen overflow-scroll rounded-lg ">
          <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
           
          <!-- validamos si hay registros -->
          <div v-if="pacientes.length > 0">
            <p class="text-lg mt-5 text-center mb-10">
              Informacion de
              <span class="text-indigo-600 font-bold">Pacientes</span>
            </p>
            
            <Paciente 
              v-for="paciente in pacientes"
              :paciente="paciente"
              @actualizar-paciente = "actualizarPaciente"
              @eliminar-paciente = "eliminarPaciente"
            />


          </div>
          <p v-else class="mt-20 text-2xl text-center">No ahi pacientes</p>


        </div>
        
    </div>
  </div>
</template>

