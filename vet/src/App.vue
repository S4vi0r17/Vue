<script setup>
import { ref, reactive, watch, onMounted } from 'vue';

import { uid } from 'uid';

import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Patient from './components/Patient.vue';

const patients = ref([]);

const patient = reactive({
  id: null,
  name: '',
  owner: '',
  email: '',
  release: '',
  symptoms: ''
});

watch(patients, () => {
  addLocalStorage();
},
  {
    deep: true // To watch nested properties
  });

const addLocalStorage = () => {
  localStorage.setItem('patients', JSON.stringify(patients.value));
}

onMounted(() => {
  const patientsLocalStorage = JSON.parse(localStorage.getItem('patients'));
  if (patientsLocalStorage) {
    patients.value = patientsLocalStorage;
  }
});

const addPatient = () => {

  if (patient.id) {
    const { id } = patient;
    const index = patients.value.findIndex(patient => patient.id === id);
    patients.value[index] = { ...patient };
  } else {
    patients.value.push({ ...patient, id: uid() });
  }



  // Reset the form
  // patient.name = '';
  // patient.owner = '';
  // patient.email = '';
  // patient.release = '';
  // patient.symptoms = '';

  // Other way to reset the form
  Object.assign(patient, {
    name: '',
    owner: '',
    email: '',
    release: '',
    symptoms: '',
    id: null
  });
}

const updatePatient = (id) => {
  const patientToUpdate = patients.value.find(patient => patient.id === id);
  // console.log(patientToUpdate, id);
  Object.assign(patient, patientToUpdate);
}

const deletePatient = (id) => {
  patients.value = patients.value.filter(patient => patient.id !== id);
}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Form v-model:name="patient.name" v-model:owner="patient.owner" v-model:email="patient.email"
        v-model:release="patient.release" v-model:symptoms="patient.symptoms" @addPatient="addPatient" :id="patient.id" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Manage your Patients</h3>

        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            <span class="text-indigo-600 font-bold">patient</span>
            information
          </p>
          <Patient v-for="(patient, index) in patients" :patient="patient" @update-patient="updatePatient"
            @delete-patient="deletePatient" />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No Patients</p>

      </div>
    </div>
  </div>
</template>