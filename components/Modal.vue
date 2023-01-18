<template>
    <div :class="`fixed inset-0 z-40 overflow-y-auto ${status ? 'block' : 'hidden'}`"
        aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div class="flex items-end justify-center min-h-screen pt-4 pb-0 text-center sm:block sm:p-0">
            <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true"></div>
            <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
            <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full w-full"
                role="dialog" aria-modal="true" aria-labelledby="modal-headline">
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6">
                    <h1 class="text-lg font-bold text-gray-700 mb-2">{{ title }}</h1>
                    <slot />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['title', 'status'],
    data() {
        return {
            //
        }
    }
}
</script>