<script setup>
import { reactive } from 'vue'; // Datos que no sean objetos(ref)
import Alert from './Alert.vue';

const alert = reactive({
    type: '',
    message: ''
});

const emit = defineEmits(['update:name', 'update:owner', 'update:email', 'update:release', 'update:symptoms', 'addPatient']);
const props = defineProps({
    name: {
        type: String,
        required: true
    },
    owner: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    release: {
        type: String,
        required: true
    },
    symptoms: {
        type: String,
        required: true
    }
});

const validate = () => {

    if (Object.values(props).includes('')) {
        alert.message = 'All fields are required';
        alert.type = 'error';
        return;
    }

    alert.message = 'Patient added successfully';
    alert.type = 'success';

    setTimeout(() => {
        // alert.message = '';
        // alert.type = '';
        Object.assign(alert, {
            message: '',
            type: ''
        });
    }, 3000);

    emit('addPatient');
}
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Patient Monitoring</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Add patients and
            <span class="text-indigo-600 font-bold">manage their information</span>
        </p>

        <Alert v-if="alert.message" :alert="alert" :message="message" />

        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validate">
            <div class="mb-5">
                <label for="petName" class="block text-gray-700 uppercase font-bold">
                    Pet's Name
                </label>
                <input id="petName" type="text" placeholder="Enter pet's name"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    :value="name" @input="$emit('update:name', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="ownerName" class="block text-gray-700 uppercase font-bold">
                    Owner's Name
                </label>
                <input id="ownerName" type="text" placeholder="Enter owner's name"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    :value="owner" @input="$emit('update:owner', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Email Address
                </label>
                <input id="email" type="email" placeholder="Enter email"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    :value="email" @input="$emit('update:email', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="releaseDate" class="block text-gray-700 uppercase font-bold">
                    Pet's Release Date
                </label>
                <input id="releaseDate" type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    :value="release" @input="$emit('update:release', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="symptoms" class="block text-gray-700 uppercase font-bold">
                    Describe Pet's Symptoms
                </label>
                <textarea id="symptoms" placeholder="Enter pet's symptoms"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-20 focus:outline-none focus:ring-2 focus:ring-indigo-500"
                    :value="symptoms" @input="$emit('update:symptoms', $event.target.value)"></textarea>
            </div>

            <input type="submit" value="Add Patient"
                class="bg-indigo-600 p-3 w-full text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors">
        </form>
    </div>
</template>