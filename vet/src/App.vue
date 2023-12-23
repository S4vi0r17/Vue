<script setup>
import { ref, reactive } from 'vue';
import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Patient from './components/Patient.vue';

const patients = ref([]);

const patient = reactive({
  name: '',
  owner: '',
  email: '',
  release: '',
  symptoms: ''
});

const addPatient = () => {
  patients.value.push({ ...patient });

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
    symptoms: ''
  });
}

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Form v-model:name="patient.name" v-model:owner="patient.owner" v-model:email="patient.email"
        v-model:release="patient.release" v-model:symptoms="patient.symptoms" @addPatient="addPatient" />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Manage your Patients</h3>

        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            <span class="text-indigo-600 font-bold">patient</span>
            information
          </p>
          <Patient v-for="(patient, index) in patients" :patient="patient" />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No Patients</p>

      </div>
    </div>
  </div>
</template>
<!-- Ver video 25 -->