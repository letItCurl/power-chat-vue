<template>
    <div class="new-message">
        <form @submit.prevent="addMessage">
            <label for="new-message">New message (hit enter to add)</label>
            <input type="text" name="new-message" v-model="newMessage">
            <p class="red-text" v-if="feedback">{{feedback}}</p>
        </form>
    </div>
</template>

<script>
import db from '@/firebase/init'
export default {
    name: 'NewMessage',
    props: ["name"],
    data(){
        return{
            newMessage: null,
            feedback: null
        }
    },
    methods: {
        addMessage(){
            if(this.newMessage){
                db.collection('messages').add({
                    content: this.newMessage,
                    name: this.name,
                    timestamp: Date.now()
                }).catch(e => console.log(e))
                this.newMessage = null
                this.feedback = null
            }else{
                this.feedback = "you must enter a message"
            }
        }
    }
}
</script>