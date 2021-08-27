<template>
    <div>
        <q-select v-model="request_type" :options="request_options" />
        <q-input v-model="request_url" type="url" placeholder="https://www.example.com"/>
        <q-btn color="primary" label="Send" @click="getRequestUrl" />
    </div>
</template>

<script>
export default {
    name: 'home',
    data() {
        return {
            request_type: { label: 'GET', value: 'GET' },
            request_url: '',
            request_options: [
                {
                    label: 'GET',
                    value: 'GET'
                },
                {
                    label: 'POST',
                    value: 'POST'
                },
                {
                    label: 'PUT',
                    value: 'PUT'
                },
                {
                    label: 'OPTIONS',
                    value: 'OPTIONS'
                },
                {
                    label: 'DELETE',
                    value: 'DELETE'
                },
            ]
        }
    },
    created() {
        console.log('Home page');
    },
    methods: {
        getRequestUrl() {
            console.log("Got URL: ", this.request_type, " | ", this.request_url);
        }
    }
}
</script>