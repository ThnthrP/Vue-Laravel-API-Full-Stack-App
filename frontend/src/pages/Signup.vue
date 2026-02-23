<script setup>
import GuessLayout from '../components/GuessLayout.vue';
import { ref } from 'vue';
import axios from 'axios'
import axiosClient from '../axiosClient';
import router from '../router';

const data = ref({
    name: '',
    email: '',
    password: '',
    password_confirmation: '',
})

const errors = ref({
    name: [],
    email: [],
    password: [],
})

// Show usages
function submit() {
    // errors.value = {}

    axiosClient.get('/sanctum/csrf-cookie').then(() => {

        axiosClient.post('/register', data.value)
            .then(() => {
                router.push({ name: 'Home' })
            })
            .catch(error => {
                console.log(error.response.data)
                errors.value = error.response.data.errors;
            })

    })
}
// async function submit() {
//     // 1) ขอ CSRF cookie (ต้องใช้ axios ปกติ)
//     await axiosClient.get('/sanctum/csrf-cookie', {
//         withCredentials: true,
//     })

//     // 2) register ผ่าน api
//     //   await axiosClient.post('/register', data.value)
//     await axiosClient.post('/register', data.value, {
//         withCredentials: true,
//     })

// }

</script>

<template>
    <GuessLayout>
        <!-- <pre>{{ data }}</pre> -->
        <h2 class="mt-10 text-center text-2xl/9 font-bold tracking-tight text-gray-900 dark:text-white">Create new
            Account</h2>
        <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
            <form @submit.prevent="submit" class="space-y-6">
                <div>
                    <!-- <label for="email" class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Email address</label> -->
                    <label for="name" class="block text-left text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                        Full Name
                    </label>
                    <div class="mt-2">
                        <input type="name" name="name" id="email" v-model="data.name"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                        <p class="text-sm mt-1 text-red-600">
                            {{ errors.name ? errors.name[0] : '' }}
                        </p>
                    </div>
                </div>

                <div>
                    <!-- <label for="email" class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Email address</label> -->
                    <label for="email" class="block text-left text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                        Email address
                    </label>
                    <div class="mt-2">
                        <input type="email" name="email" id="email" autocomplete="email" v-model="data.email"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                        <p class="text-sm mt-1 text-red-600">
                            {{ errors.email ? errors.email[0] : '' }}
                        </p>
                    </div>
                </div>
                <div>
                    <div class="flex items-center justify-between">
                        <label for="password"
                            class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Password</label>
                        <!-- <div class="text-sm">
                            <a href="#"
                                class="font-semibold text-indigo-600 hover:text-indigo-500 dark:text-indigo-400 dark:hover:text-indigo-300">Forgot
                                password?</a>
                        </div> -->

                    </div>
                    <div class="mt-2">
                        <input type="password" name="password" id="password" v-model="data.password"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                        <p class="text-sm mt-1 text-red-600">
                            {{ errors.password ? errors.password[0] : '' }}
                        </p>
                    </div>
                </div>

                <div>
                    <div class="flex items-center justify-between">
                        <label for="passwordConfirmation"
                            class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Repeat Password</label>
                    </div>
                    <div class="mt-2">
                        <input type="password" name="password_confirmation" id="passwordConfirmation"
                            v-model="data.password_confirmation"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                    </div>
                </div>

                <div>
                    <button type="submit"
                        class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm/6 font-semibold text-white shadow-xs hover:bg-indigo-500 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 dark:bg-indigo-500 dark:shadow-none dark:hover:bg-indigo-400 dark:focus-visible:outline-indigo-500">

                        Create an Account</button>
                </div>
            </form>

            <p class="mt-10 text-center text-sm/6 text-gray-500 dark:text-gray-400">
                Already have an account?
                {{ ' ' }}
                <router-link :to="{ name: 'Login' }"
                    class="font-semibold text-indigo-600 hover:text-indigo-500 dark:text-indigo-400 dark:hover:text-indigo-300">
                    Login from here</router-link>
            </p>
        </div>
    </GuessLayout>

</template>

<style scoped></style>