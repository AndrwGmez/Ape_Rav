<template>
	<div class="min-h-screen p-4 flex flex-col  -mt-20">
    <!-- Sección Superior (Título, Icono, Información del Ciudadano) -->
		<div class="flex flex-col md:flex-row items-center justify-between p-2 rounded-lg w-full max-w-7xl mb-1">
      <!-- Icono y Título -->
			<div class="flex items-center mb-4 md:">
        <div class="p-6 bg-azulBarraApe rounded-full">
          <!-- Icono SVG -->
          <img :src="lineaAten" 
                alt=" Icono de Lineas de atencion" 
                class="w-16 h-16 "/>
        </div>
        <div class="ml-4 text-center md:text-left mt-14">
          <p class="text-black -mb-1 text-2xl md:text-[37px]">Lineas de</p>
          <h2 class="text-customPurple text-5xl md:text-[94px] mt-0 font-bold">Atención</h2>
        </div>
      </div>
    </div>

    <!-- Contenedores de imagen -->
    <div class="md:flex-col md:gap-7 md:p-4  grid grid-col gap-2">
      <div class="flex items-center space-x-1 flex-auto -mb-5">
        <img :src="select1" alt="Icono de selección 1" class="w-5 h-5 " />
        <p class="font-sans text-sm md:text-xl">Seleccione la</p>
        <h3 class="font-sans text-sm md:text-xl">Entidad</h3>
      </div>
      <div class="md:flex md:justify-between grid grid-cols-2  items-center gap-x-3 gap-y-8 p-2 ">
        <div @click="selectImage(LogoSenaverde); selectSede(sedes[0])"
          class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer md:p-2 shadow-md h-24 w-42 justify-center"
          :class="['', selectedImage === LogoSenaverde ? 'bg-gray-200' : 'bg-white']">
          <img :src="LogoSenaverde" alt="imagen logo verde sena"
            class="md:w-20 md:h-40 object-center md:-mt-1 w-40 h-16 mt-4" />
          <h4 class="font-bold md:-mb-10 text-center text-sm md:text-xl ">SENA</h4>
        </div>
        <div @click="selectImage(LogoApe); selectSede(sedes[1])"
          class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer  shadow-md h-24 w-40 "
          :class="[selectedImage === LogoApe ? 'bg-gray-200' : 'bg-white']">
          <img :src="LogoApe" alt="Imagen ape" class="object-scale-down md:h-48 md:w-64 w-44 h-12 mt-7 " />
          <h4 class="font-bold md:-mb-10 text-center text-sm md:text-xl ">APE</h4>
        </div>
        <div @click="selectImage(LogCertLab); selectSede(sedes[2])"
          class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer md:p-2 shadow-md h-24 w-42 px-2"
          :class="[selectedImage === LogCertLab ? 'bg-gray-200' : 'bg-white']">
          <img :src="LogCertLab" alt="Imagen certificados laborales"
            class="object-scale-down md:h-48 md:w-64  w-36 h-5 mt-10 mb-4" />
          <h4 class="font-bold md:-mb-10 text-center text-sm md:text-xl -mb-7">CERTIFICACIONES</h4>
        </div>
        <div @click="selectImage(FondoEmprender); selectSede(sedes[3])"
          class="md:w-72 md:h-36 md:flex md:flex-col items-center rounded-lg cursor-pointer p-2 shadow-md h-24 w-42"
          :class="[selectedImage === FondoEmprender ? 'bg-gray-200' : 'bg-white']">
          <img :src="FondoEmprender" alt="Imagen fondo emprender"
            class="object-scale-down md:h-48 md:w-64 object-center w-36 h-7 mt-8 mb-2" />
          <h4 class="font-bold md:-mb-10 text-center text-sm md:text-xl -mb-7">FONDO EMPRENDER</h4>
        </div>
      </div>

      <!-- Selección de Regional -->
      <div class="flex items-center space-x-1 flex-auto ">
        <img :src="select2" alt="Icono de selección 2" class="h-5 w-5 " />
        <p class="font-sans text-sm md:text-xl">Seleccione la</p>
        <h3 class="font-sans text-sm md:text-xl">Regional</h3>
      </div>
      <div class="flex items-center space-x-2 h-1 shadow-md">
        <!-- Selección de Departamento -->
        <Select v-model="selectedCountry" :options="departamentos" placeholder="Seleccione departamento"
          class="w-full rounded-lg shadow-md text-black" @change="updateSelectedInfo">
          <template #value="slotProps">
            <div v-if="slotProps.value" class="flex gap-2 items-center font-semibold text-black">
              <div>{{ slotProps.value.name }}</div>
            </div>
            <span v-else class="text-azulBarraApe">{{ slotProps.placeholder }}</span>
          </template>
          <template #option="slotProps">
            <div class="flex items-center font-medium text-azulBarraApe">
              <p>{{ slotProps.option.name }}</p>
            </div>
          </template>
        </Select>
      </div>
      <div class="flex items-center justify-center">
        <!-- Botón de Búsqueda -->
        <Button
          class="md:w-96  w-full h-10 !bg-azulBarraApe text-lg cursor-pointer border-none !text-amarillo font-bold py-2 rounded-lg shadow-md mt-4"
          type="button" label="Buscar" icon="" :loading="loading" @click="openModal" />
      </div>
    </div>

    <!-- Componente de PopUp -->
    <PopUp :isOpen="isModalOpen" :image="selectedImage" :sede="selectedSede" :departamento="selectedCountry?.name"
      @close="isModalOpen = false" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import select1 from "@/assets/imgLineasAtencion/select1.svg";
import select2 from "@/assets/imgLineasAtencion/select2.svg";
import lineaAten from "@/assets/imgLineasAtencion/lineaAten.svg";
import LogoSenaverde from "@/assets/images/logosenaverde.svg";
import LogCertLab from "@/assets/imgLineasAtencion/LogCertLab.svg";
import LogoApe from "@/assets/images/logoape.png";
import FondoEmprender from "@/assets/images/fondoemprender.png";
import Select from "primevue/select";
import PopUp from './PopUp.vue';

const selectedCountry = ref();
const selectedInfo = ref(null);
const isModalOpen = ref(false);
const loading = ref(false);
const selectedImage = ref(null);
const selectedSede = ref(null);
const selectedDepart = ref(null);

const sedes = ["SERVICIO NACIONAL DE APRENDISAJE SENA", "AGENCIA PUBLICA DE EMPLEO", "CERTIFICACION COMPETENCIAS LABORALES", "FONDO EMPRENDER"];



const departamentos = ref([
  { name: "Amazonas", code: "91" },
  { name: "Antioquia", code: "05" },
  { name: "Arauca", code: "81" },
  { name: "Atlantico", code: "08" },
  { name: "Bolivar", code: "13" },
  { name: "Boyacá", code: "15" },
  { name: "Caldas", code: "17" },
  { name: "Caquetá", code: "18" },
  { name: "Casanare", code: "85" },
  { name: "Cauca", code: "19" },
  { name: "Cesar", code: "20" },
  { name: "Chocó", code: "27" },
  { name: "Cundinamarca", code: "25" },
  { name: "Cordoba", code: "23" },
  { name: "Guainia", code: "94" },
  { name: "Guaviare", code: "95" },
  { name: "Huila", code: "41" },
  { name: "La Guajira", code: "44" },
  { name: "Magdalena", code: "47" },
  { name: "Meta", code: "50" },
  { name: "Nariño", code: "52" },
  { name: "Norte de Santander", code: "54" },
  { name: "Putumayo", code: "86" },
  { name: "Quindio", code: "63" },
  { name: "Risaralda", code: "66" },
  { name: "San Andres, Providencia y Santa Catalina", code: "88" },
  { name: "Santander", code: "68" },
  { name: "Sucre", code: "70" },
  { name: "Tolima", code: "73" },
  { name: "Valle del Cauca", code: "76" },
  { name: "Vaupés", code: "97" },
  { name: "Vichada", code: "99" },
]);



const updateSelectedInfo = () => {
  const selected = departamentos.value.find((dep) => dep.code === selectedCountry.value?.code);
  selectedInfo.value = selected ? selected : null;
  console.log(selectedInfo.value);
};

const selectImage = (image) => {
  selectedImage.value = image;
};

const selectSede = (sede) => {
  selectedSede.value = sede;
};
const selectDepart = (departamentos) => {
  selectedDepart.value = sede;
};


const openModal = () => {
  isModalOpen.value = true;
};
</script>

<style scoped>
/* No additional CSS needed as everything is styled with Tailwind CSS */
</style>
