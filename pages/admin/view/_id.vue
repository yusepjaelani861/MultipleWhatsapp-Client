<template>
    <AdminLayout>
        <h1 class="text-3xl font-bold mb-4">
            View Client {{ client.name }}
        </h1>

        <div class="p-4">
            <div v-if="client.status === 'ACTIVE'" class="flex items-center justify-end mb-4">
                <button @click="modal = true"
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                    <a>Try Send Message</a>
                </button>
            </div>

            <div class="flex">
                <div class="w-full bg-white dark:bg-gray-900 rounded-lg shadow-lg p-4">
                    <table class="min-w-full bg-white dark:bg-gray-900 font-bold">
                        <tbody v-if="!loading" class="text-gray-700 dark:text-gray-400">
                            <tr class="text-center">
                                <td class="w-1/12 py-3 px-4">Name</td>
                                <td class="w-2/12 py-3 px-4">{{ client.name }}</td>
                            </tr>
                            <tr class="text-center">
                                <td class="w-1/12 py-3 px-4">Status</td>
                                <td class="w-2/12 py-3 px-4"
                                    :class="client.status === 'ACTIVE' ? 'text-green-500' : 'text-red-500'">{{
                                        client.status
                                    }}</td>
                            </tr>
                            <tr class="text-center">
                                <td class="w-1/12 py-3 px-4">Created</td>
                                <td class="w-2/12 py-3 px-4">{{ formatDate(client.created_at) }}</td>
                            </tr>
                        </tbody>
                        <tbody v-else class="text-gray-700 dark:text-gray-400">
                            <tr class="text-center" colspan="2">
                                <td class="w-1/12 py-3 px-4">Loading...</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <div class="flex-shrink-0 w-1/3 ml-4">
                    <div class="bg-white dark:bg-gray-900 rounded-lg shadow-lg p-4">
                        <div v-if="!loading" class="flex flex-col items-center justify-center">
                            <div v-if="client.status === 'INACTIVE'" class="bg-gray-300 rounded-lg mb-2"
                                :class="client.qr_url ? 'p-2' : 'p-4'">
                                <img v-if="client.qr_url" class="w-full rounded-lg object-cover"
                                    :src="$config.API_URL + '/public/' + client.name + '-qr.png'" alt="avatar">
                                <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                    stroke-width="1.5" stroke="currentColor" class="w-full">
                                    <path stroke-linecap="round" stroke-linejoin="round"
                                        d="M2.25 15.75l5.159-5.159a2.25 2.25 0 013.182 0l5.159 5.159m-1.5-1.5l1.409-1.409a2.25 2.25 0 013.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 001.5-1.5V6a1.5 1.5 0 00-1.5-1.5H3.75A1.5 1.5 0 002.25 6v12a1.5 1.5 0 001.5 1.5zm10.5-11.25h.008v.008h-.008V8.25zm.375 0a.375.375 0 11-.75 0 .375.375 0 01.75 0z" />
                                </svg>
                                <p v-if="!client.qr_url"
                                    class="text-xl font-bold text-gray-900 dark:text-gray-100 ml-4">Loading...</p>
                            </div>
                            <p class="text-xl font-bold text-gray-900 dark:text-gray-100 ml-4">Status <span
                                    :class="client.status === 'ACTIVE' ? 'text-green-500' : 'text-red-500'">
                                    {{ client.status }}</span></p>
                        </div>
                        <div v-else class="flex flex-col items-center justify-center">
                            <div class="p-4 bg-gray-300 rounded-lg mb-2">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                    stroke-width="1.5" stroke="currentColor" class="w-full">
                                    <path stroke-linecap="round" stroke-linejoin="round"
                                        d="M2.25 15.75l5.159-5.159a2.25 2.25 0 013.182 0l5.159 5.159m-1.5-1.5l1.409-1.409a2.25 2.25 0 013.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 001.5-1.5V6a1.5 1.5 0 00-1.5-1.5H3.75A1.5 1.5 0 002.25 6v12a1.5 1.5 0 001.5 1.5zm10.5-11.25h.008v.008h-.008V8.25zm.375 0a.375.375 0 11-.75 0 .375.375 0 01.75 0z" />
                                </svg>
                            </div>

                            <p class="text-xl font-bold text-gray-900 dark:text-gray-100 ml-4">Loading...</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <Modal title="Create New Client" :status="modal">
            <div class="bg-gray-100 p-2 px-4 border rounded-lg">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="number">
                    Number <span class="text-xs ml-4 text-red-500">*</span> <span class="text-xs text-gray-500">Please
                        using format 628123456789</span>
                </label>
                <input v-model="number"
                    class="shadow appearance-none border mb-2 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="number" type="text" placeholder="628xxxxxxxx">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="message">
                    Message
                </label>
                <input v-model="message"
                    class="shadow appearance-none border mb-2 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="message" type="text" placeholder="Message">
            </div>

            <div class="flex justify-end mt-4">
                <button type="button" @click="modal = false, number = '', message = ''"
                    class="inline-flex justify-center px-4 py-2 text-sm font-medium text-white bg-gray-500 border border-transparent rounded-md hover:bg-gray-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-gray-500">
                    Cancel
                </button>
                <button type="button" @click="sendMessage"
                    class="inline-flex ml-2 justify-center px-4 py-2 text-sm font-medium text-white bg-blue-500 border border-transparent rounded-md hover:bg-blue-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-gray-500">
                    {{ process_send? 'Loading ...': 'Send' }}
                </button>
            </div>
        </Modal>

        <Notification :notification="notification" :message="error.message" :error="!error.success" />
    </AdminLayout>
</template>

<script>
import axios from 'axios';
import AdminLayout from '../../../layouts/AdminLayout.vue';
import Modal from '../../../components/Modal.vue';
import Notification from '../../../components/Notification.vue';


export default {
    data() {
        return {
            token: null,
            client: {
                name: '',
                status: '',
                qr_url: '',
                created_at: '',
            },
            loading: false,
            number: '',
            message: '',
            modal: false,
            process_send: false,
            error: {
                message: '',
                success: false,
            },
            notification: false,
        };
    },
    asyncData({ isDev, route, store, env, params, query, req, res, redirect, error }) {
        const id = params.id;
        return {
            id
        };
    },
    async mounted() {
        const api_url = this.$config.API_URL;
        this.loading = true;
        this.token = document.cookie.split(";").find(row => row.trim().startsWith("token=")).split("=")[1];
        if (!this.token) {
            window.location.href = "/";
        }
        try {
            const response = await axios.post(`${api_url}/api/v1/auth/check`, {}, {
                headers: {
                    "Authorization": `Bearer ${this.token}`
                }
            });
            if (response.data.success !== true) {
                window.location.href = "/";
            }

            setTimeout(async () => {
                const response2 = await axios.get(`${api_url}/api/v1/client/${this.id}`, {
                    headers: {
                        "Authorization": `Bearer ${this.token}`
                    }
                })

                if (response2.data.success !== true) {
                    window.location.href = "/";
                }
                this.client = response2.data.data;
                this.loading = false;
            }, 1000);

            setInterval(async () => {
                const response2 = await axios.get(`${api_url}/api/v1/client/${this.id}`, {
                    headers: {
                        "Authorization": `Bearer ${this.token}`
                    }
                })

                if (response2.data.success !== true) {
                    window.location.href = "/";
                }
                this.client = response2.data.data;
            }, 5000);
        }
        catch (error) {
            // console.log(error);
            this.loading = false;
            this.error = error?.response?.data
            this.notification = true;
            setTimeout(() => {
                this.notification = false;
            }, 1000);
            if (error.response?.data?.success === false) {
                document.cookie = `token=; path=/; expires=Thu, 01 Jan 1970 00:00:00 UTC;`;
                window.location.href = "/";
            }
        }
    },
    methods: {
        formatDate(date) {
            const d = new Date(date);
            const month = d.toLocaleString('default', { month: 'short' });
            const day = d.getDate();
            const year = d.getFullYear();
            const hour = d.getHours();
            const min = d.getMinutes();
            const ampm = hour >= 12 ? 'PM' : 'AM';
            return `${month} ${day}, ${year} ${hour}:${min} ${ampm}`;
        },

        async sendMessage() {
            this.process_send = true;
            try {
                const response = await axios.post(`${this.$config.API_URL}/api/v1/whatsapp/send`, {
                    client_id: this.client.name,
                    to: this.number,
                    message: this.message
                }, {
                    headers: {
                        "Authorization": `Bearer ${this.token}`
                    }
                })

                if (response.data.success === true) {
                    this.process_send = false;
                    this.modal = false;
                    this.number = '';
                    this.message = '';
                }
            } catch (error) {
                this.process_send = false;
                this.modal = false;
                this.number = '';
                this.message = '';
                this.error = error?.response?.data
                this.notification = true;
                setTimeout(() => {
                    this.notification = false;
                }, 1000);
            }
        }
    },
    components: { AdminLayout, Modal }
}
</script>