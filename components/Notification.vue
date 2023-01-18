
<template>
    <div class="fixed top-4 z-50 rounded-lg border text-white p-4"
        :class="`transition duration-500 ease-in-out transform ${notification ? 'right-4 translate-x-0' : 'translate-x-full right-0'} ${error ? 'bg-red-500' : 'bg-green-500'}`">
        <h1 class="text-lg font-bold">{{ message }}</h1>
    </div>
</template>

<script>
export default {
    props: ["notification", "message", "error"],
    data() {
        return {
            //
        }
    }
}
</script>