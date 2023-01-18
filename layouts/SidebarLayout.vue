<template>
    <div class="fixed z-30 w-64 justify-center p-4 bg-slate-800 dark:bg-slate-900 border-r border-gray-500 h-full">
        <a href="/admin/dashboard">
            <h1 class="text-3xl font-bold dark:text-gray-900 text-center text-gray-100 mb-4">
                Whatsapp Comun
            </h1>
        </a>

        <div class="space-y-8">
            <ul class="mt-3">
                <!-- Dashboard -->
                <li class="px-3 py-2 rounded-sm mb-2 last:mb-0 hover:bg-slate-700"
                    :class="$route.path === '/admin/dashboard' ? 'bg-slate-700' : ''">
                    <a href="/admin/dashboard" class="block text-slate-200 hover:text-white truncate transition duration-150">
                        <div class="flex items-center">
                            <svg xmlns="http://www.w3.org/2000/svg" class="shrink-0 h-6 w-6" fill="none"
                                viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                <path stroke-linecap="round" stroke-linejoin="round"
                                    d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z" />
                            </svg>
                            <span class="text-md font-bold text-white dark:text-slate-200 ml-4">Dashboard</span>
                        </div>
                    </a>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            //
        }
    },
    components: {},
    created() {
        // console log path route
    }
}
</script>