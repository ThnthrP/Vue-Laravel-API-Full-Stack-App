<script setup>

import { Menu, MenuButton, MenuItems, MenuItem } from '@headlessui/vue'
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/vue'
import axiosClient from "../axiosClient.js";
// import router from '../router.js';
import { useRouter } from 'vue-router'
import useUserStore from "../store/user.js";
import { computed } from "vue";

const userStore = useUserStore()

const user = computed(() => userStore.user)

const router = useRouter()

const navigation = [
    { name: 'Upload', to: { name: 'Home' } },
    { name: 'My Images', to: { name: 'MyImages' } },
]

// const logout = () => {
//     console.log("logout")
// }

function logout() {
    axiosClient.post('/logout')
        .catch(() => {
            // ignore 401
        })
        .finally(() => {
            router.push({ name: 'Login' })
        })
}

</script>

<template>
    <!-- Include this script tag or install `@tailwindplus/elements` via npm: -->
    <!-- <script src="https://cdn.jsdelivr.net/npm/@tailwindplus/elements@1" type="module"></script> -->
    <nav class="relative bg-gray-800">
        <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
            <div class="relative flex h-16 items-center justify-between">
                <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
                    <!-- Mobile menu button-->
                    <button type="button" command="--toggle" commandfor="mobile-menu"
                        class="relative inline-flex items-center justify-center rounded-md p-2 text-gray-400 hover:bg-white/5 hover:text-white focus:outline-2 focus:-outline-offset-1 focus:outline-indigo-500">
                        <span class="absolute -inset-0.5"></span>
                        <span class="sr-only">Open main menu</span>
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" data-slot="icon"
                            aria-hidden="true" class="size-6 in-aria-expanded:hidden">
                            <path d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" stroke-linecap="round"
                                stroke-linejoin="round" />
                        </svg>
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" data-slot="icon"
                            aria-hidden="true" class="size-6 not-in-aria-expanded:hidden">
                            <path d="M6 18 18 6M6 6l12 12" stroke-linecap="round" stroke-linejoin="round" />
                        </svg>
                    </button>
                </div>
                <div class="flex flex-1 items-center justify-center sm:items-stretch sm:justify-start">
                    <div class="flex shrink-0 items-center">
                        <img src="https://tailwindcss.com/plus-assets/img/logos/mark.svg?color=indigo&shade=500"
                            alt="Your Company" class="h-8 w-auto" />
                    </div>
                    <div class="hidden sm:ml-6 sm:block">
                        <div class="flex space-x-4">
                            <!-- Current: "bg-gray-900 text-white", Default: "text-gray-300 hover:bg-white/5 hover:text-white" -->
                            <RouterLink to="/" class="rounded-md bg-gray-900 px-3 py-2 text-sm font-medium text-white">
                                Upload</RouterLink>
                            <RouterLink to="/images" class="rounded-md px-3 py-2 text-sm font-medium text-gray-300 
                                hover:bg-white/5 hover:text-white">My Images</RouterLink>

                        </div>
                    </div>
                </div>
                <div class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
                    <button type="button"
                        class="relative rounded-full p-1 text-gray-400 focus:outline-2 focus:outline-offset-2 focus:outline-indigo-500">
                        <span class="absolute -inset-1.5"></span>
                        <span class="sr-only">View notifications</span>
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" data-slot="icon"
                            aria-hidden="true" class="size-6">
                            <path
                                d="M14.857 17.082a23.848 23.848 0 0 0 5.454-1.31A8.967 8.967 0 0 1 18 9.75V9A6 6 0 0 0 6 9v.75a8.967 8.967 0 0 1-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 0 1-5.714 0m5.714 0a3 3 0 1 1-5.714 0"
                                stroke-linecap="round" stroke-linejoin="round" />
                        </svg>
                    </button>

                    <!-- Profile dropdown -->
                    <Menu as="div" class="relative ml-3">
                        <div>
                            <MenuButton class="relative flex rounded-full focus:outline-none">
                                <!-- <img class="h-8 w-8 rounded-full"
                                    src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e" /> -->
                                <img class="size-8 rounded-full" src="https://randomuser.me/api/portraits/men/81.jpg"
                                    alt="" />
                                <span class="text-white ml-3">{{ user.name }}</span>
                            </MenuButton>
                        </div>

                        <MenuItems
                            class="absolute right-0 z-10 mt-2 w-48 origin-top-right rounded-md bg-white py-1 shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
                            <MenuItem v-slot="{ active }">
                            <button @click="logout" :class="[
                                active ? 'bg-gray-100' : '',
                                'block w-full px-4 py-2 text-left text-sm text-gray-700'
                            ]">
                                Sign out
                            </button>
                            </MenuItem>
                        </MenuItems>
                    </Menu>
                </div>
            </div>
        </div>

        <Disclosure as="div" class="sm:hidden">
            <DisclosurePanel class="space-y-1 px-2 pt-2 pb-3">
                <!-- Current: "bg-gray-900 text-white", Default: "text-gray-300 hover:bg-white/5 hover:text-white" -->
                <a href="#" aria-current="page"
                    class="block rounded-md bg-gray-900 px-3 py-2 text-base font-medium text-white">Dashboard</a>
                <a href="#"
                    class="block rounded-md px-3 py-2 text-base font-medium text-gray-300 hover:bg-white/5 hover:text-white">Team</a>
                <a href="#"
                    class="block rounded-md px-3 py-2 text-base font-medium text-gray-300 hover:bg-white/5 hover:text-white">Projects</a>
                <a href="#"
                    class="block rounded-md px-3 py-2 text-base font-medium text-gray-300 hover:bg-white/5 hover:text-white">Calendar</a>
            </DisclosurePanel>
        </Disclosure>
    </nav>

    <router-view />


</template>

<style scoped></style>