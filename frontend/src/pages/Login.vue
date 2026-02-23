<script setup>
import GuessLayout from '../components/GuessLayout.vue';
import axiosClient from "../axiosClient.js";
import { ref } from "vue";
import router from "../router.js";

const data = ref({
    email: '',
    password: '',
})

const errorMessage = ref('')

function submit() {
    axiosClient.get('/sanctum/csrf-cookie').then(response => {
        // axiosClient.post("/login", data.value)
        //     .then(response => {
        //       router.push({name: 'Home'})
        //     })
        //     .catch(error => {
        //       console.log(error.response)
        //       errorMessage.value = error.response.data.message;
        //     })
        axiosClient.post("/login", data.value)
            .then(() => {
                return axiosClient.get('/api/user')
            })
            .then((res) => {
                // console.log(res.data)
                router.push({ name: 'Home' })
            })
            .catch(error => {
                // console.log(error.response)
                errorMessage.value = error.response.data.message;
            })
    });
}



</script>

<template>
    <GuessLayout>
        <h2 class="mt-10 text-center text-2xl/9 font-bold tracking-tight text-gray-900 dark:text-white">Sign in to your
            account</h2>
        <div v-if="errorMessage" class="mt-4 py-2 px-3 rounded text-white bg-red-400">
            {{ errorMessage }}
        </div>
        <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
            <form @submit.prevent="submit" class="space-y-6">
                <div>
                    <!-- <label for="email" class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Email address</label> -->
                    <label for="email" class="block text-left text-sm/6 font-medium text-gray-900 dark:text-gray-100">
                        Email address
                    </label>
                    <div class="mt-2">
                        <input type="email" name="email" id="email" autocomplete="email" required=""
                            v-model="data.email"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                    </div>
                </div>

                <div>
                    <div class="flex items-center justify-between">
                        <label for="password"
                            class="block text-sm/6 font-medium text-gray-900 dark:text-gray-100">Password</label>
                        <div class="text-sm">
                            <a href="#"
                                class="font-semibold text-indigo-600 hover:text-indigo-500 dark:text-indigo-400 dark:hover:text-indigo-300">Forgot
                                password?</a>
                        </div>
                    </div>
                    <div class="mt-2">
                        <input type="password" name="password" id="password" autocomplete="current-password" required=""
                            v-model="data.password"
                            class="block w-full rounded-md bg-white px-3 py-1.5 text-base text-gray-900 outline-1 -outline-offset-1 outline-gray-300 placeholder:text-gray-400 focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600 sm:text-sm/6 dark:bg-white/5 dark:text-white dark:outline-white/10 dark:placeholder:text-gray-500 dark:focus:outline-indigo-500" />
                    </div>
                </div>

                <div>
                    <button type="submit"
                        class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm/6 font-semibold text-white shadow-xs hover:bg-indigo-500 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 dark:bg-indigo-500 dark:shadow-none dark:hover:bg-indigo-400 dark:focus-visible:outline-indigo-500">Sign
                        in</button>
                </div>
            </form>

            <p class="mt-10 text-center text-sm/6 text-gray-500 dark:text-gray-400">
                Not a member?
                {{ ' ' }}
                <router-link :to="{ name: 'Signup' }"
                    class="font-semibold text-indigo-600 hover:text-indigo-500 dark:text-indigo-400 dark:hover:text-indigo-300">
                    Create an account</router-link>
            </p>
        </div>
    </GuessLayout>
</template>

<style scoped></style>