<template>
    <AdminLayout>
        <h1 class="text-3xl font-bold text-gray-900 text-left dark:text-gray-100 mb-4">
            Dashboard
        </h1>

        <div class="p-4">
            <div class="flex items-center justify-end mb-4">
                <button @click="modal = true"
                    class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                    <a>Create Client</a>
                </button>
            </div>
            <div class="overflow-auto rounded-lg">
                <table class="min-w-full bg-white dark:bg-gray-900 font-bold">
                    <thead class="bg-gray-800 dark:bg-gray-700 text-white">
                        <tr>
                            <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">No</th>
                            <th class="w-2/12 py-3 px-4 uppercase font-semibold text-sm">Nama</th>
                            <th class="w-2/12 py-3 px-4 uppercase font-semibold text-sm">Status</th>
                            <th class="w-2/12 py-3 px-4 uppercase font-semibold text-sm"></th>
                        </tr>
                    </thead>
                    <tbody class="text-gray-700 dark:text-gray-400">
                        <tr v-if="loading" class="text-center">
                            <td class="w-2/12 py-3 px-4" colspan="4">Loading ...</td>
                        </tr>
                        <tr v-if="!loading && clients.length === 0" class="text-center">
                            <td class="w-2/12 py-3 px-4" colspan="4">No data</td>
                        </tr>
                        <tr v-else v-for="client in clients" class="text-center">
                            <td class="w-1/12 py-3 px-4">{{ client.id }}</td>
                            <td class="w-2/12 py-3 px-4">{{ client.name }}</td>
                            <td class="w-2/12 py-3 px-4"
                                :class="client.status === 'ACTIVE' ? 'text-green-500' : 'text-red-500'">{{
                                    client.status
                                }}</td>
                            <td class="w-2/12 py-3 px-4">
                                <a :href="`/admin/view/${client.id}`" class="text-blue-500 hover:text-blue-700">View</a>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>

        <Modal title="Create New Client" :status="modal">
            <div class="bg-gray-100 p-2 px-4 border rounded-lg">
                <label class="block text-gray-700 text-sm font-bold mb-2" for="name">
                    Name Client
                </label>
                <input v-model="name"
                    class="shadow appearance-none border mb-2 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="name" type="text" placeholder="Name">
            </div>

            <div class="flex justify-end mt-4">
                <button type="button" @click="modal = false, name = ''"
                    class="inline-flex justify-center px-4 py-2 text-sm font-medium text-white bg-gray-500 border border-transparent rounded-md hover:bg-gray-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-gray-500">
                    Cancel
                </button>
                <button type="button" @click="createClient"
                    class="inline-flex ml-2 justify-center px-4 py-2 text-sm font-medium text-white bg-blue-500 border border-transparent rounded-md hover:bg-blue-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-gray-500">
                    {{ process_create? 'Loading ...': 'Create' }}
                </button>
            </div>
        </Modal>
    </AdminLayout>
</template>

<script>
import axios from 'axios';
import AdminLayout from '../../layouts/AdminLayout.vue';
import Modal from '../../components/Modal.vue';

export default {
    data() {
        return {
            token: null,
            page: 1,
            clients: [],
            loading: true,
            modal: false,
            name: '',
            process_create: false,
        }
    },
    async mounted() {
        this.loading = true;
        this.token = document.cookie.split(';').find(row => row.trim().startsWith('token=')).split('=')[1];
        if (!this.token) {
            window.location.href = '/';
        }

        try {
            const response = await axios.post(`${this.$config.API_URL}/api/v1/auth/check`, {}, {
                headers: {
                    'Authorization': `Bearer ${this.token}`
                }
            })

            if (response.data.success !== true) {
                window.location.href = '/';
            }

            setTimeout(async () => {
                const response2 = await axios.get(`${this.$config.API_URL}/api/v1/client/list?page=${this.page}`, {
                    headers: {
                        'Authorization': `Bearer ${this.token}`
                    }
                })
    
                this.clients = response2.data.data
                this.page = response2.data.pagination.next_page
                this.loading = false
            }, 1000);

            setInterval(async () => {
                const response2 = await axios.get(`${this.$config.API_URL}/api/v1/client/list?page=${this.page}`, {
                    headers: {
                        'Authorization': `Bearer ${this.token}`
                    }
                })

                this.clients = response2.data.data
                this.page = response2.data.pagination.next_page
            }, 10000)

        } catch (error) {
            this.loading = false
            // console.log(error)
            if (error.response.data.success === false) {
                document.cookie = `token=; path=/; expires=Thu, 01 Jan 1970 00:00:00 UTC;`;
                window.location.href = '/';
            }
        }
    },
    methods: {
        async createClient() {
            this.process_create = true
            try {
                const response = await axios.post(`${this.$config.API_URL}/api/v1/whatsapp/generate`, {
                    name: this.name
                }, {
                    headers: {
                        "Authorization": `Bearer ${this.token}`
                    }
                })

                if (response.data.success === true) {
                    this.modal = false
                    this.name = ''
                    this.clients.push(response.data.data)
                }
            } catch (error) {
                // console.log(error)
            }


        }
    },
    components: {
        AdminLayout,
        Modal
    }
}
</script>