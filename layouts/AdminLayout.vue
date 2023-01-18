<template>
    <div class="min-h-screen bg-gray-100 dark:bg-gray-900">
        <div class="fixed z-20 w-full px-4 py-2 dark:bg-slate-900 bg-slate-800 border-b border-gray-500 shadow-md">
            <div class="flex justify-between items-center" @click="openLogout">
                <a href="/">
                    <h1 class="text-2xl font-bold text-gray-100 text-center px-4">Whatsapp Comun</h1>
                </a>
                <div class="flex items-center hover:bg-gray-700 p-2 rounded-lg">
                    <img src="https://ui-avatars.com/api/?name=Yusep Jaelani&background=0D8ABC&color=fff&size=128"
                        alt="Logo" class="w-8 h-8 rounded-full">
                    <span class="text-md font-bold text-gray-100 ml-2">Yusep Jaelani</span>
                </div>
            </div>
        </div>
        <div id="navLogout" class="fixed z-10 top-16 right-2 w-40 p-2 dark:bg-slate-900 bg-slate-800 rounded-lg"
            :class="logoutStatus ? 'block' : 'hidden'">
            <button
                class="text-sm w-full font-bold px-2 py-1 text-gray-100 hover:bg-gray-600 rounded-lg">Logout</button>
        </div>
        <SidebarLayout />
        <div class="flex h-full ml-64 pt-16">
            <!-- Page Content -->
            <main class="min-h-screen w-full dark:bg-gray-300 bg-gray-100">
                <div class="p-4">
                    <slot />
                </div>
            </main>
        </div>
    </div>
</template>

<script>
import SidebarLayout from './SidebarLayout.vue';

export default {
    data() {
        return {
            logoutStatus: false
        };
    },
    methods: {
        openLogout() {
            this.logoutStatus = !this.logoutStatus;
        }
    },
    components: { SidebarLayout }
}
</script>