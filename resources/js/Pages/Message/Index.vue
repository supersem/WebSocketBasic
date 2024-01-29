<template>
    <div class="w-1/2 mx-auto py-6">
        <div v-if="messages.length" class="mb-4">
            <div>
                <div class="mb-4">
                    <input type="text" v-model="body" class="rounded-lg border border-gray-400" placeholder="your message">
                </div>
                <div>
                    <a @click.prevent="store" href="#" class="rounded-lg block w-48 bg-sky-400 text-white text-center py-2">Send</a>
                </div>
            </div>
            <h3 class="mb-4 mt-4">Messages</h3>
            <div>
                <div class="text-sm pb-4 mb-4 mt-4 border-b border-gray-300" v-for="message in messages">
                    <p>{{ message.id }}</p>
                    <p>{{ message.body }}</p>
                    <p class="text-right"> {{ message.time }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Index",
    props: [
        'messages'
    ],
    data() {
        return {
            body: ''
        }
    },

    created() {
        window.Echo.channel('store_message')
            .listen('.store_message', res => {
                this.messages.unshift(res.message)
                this.body = ''
            })
    },

    methods: {
        store() {
            axios.post('/messages', {body: this.body})
                .then( res => {
                    this.messages.unshift(res.data)
                    this.body = ''
                })
        }
    }
}
</script>

<style scoped>

</style>
