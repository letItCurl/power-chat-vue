<template>
    <div class="chat container">
        <h2 class="teal-text">Log as <span class="red-text">{{name}}</span></h2>
        <div class="card left-align">
            <div class="card-content">
                <ul class="messages" v-chat-scroll>
                    <li v-for="message in messages" :key="message">
                        <div v-if="iam(message.name)" class="right-align iam">
                            <div class="red-text">{{message.name}}</div>
                            <div class="grey-text text-darken-3">{{message.content}}</div>
                            <div class="grey-text time">{{message.timestamp}}</div>
                        </div>
                        <div v-if="!iam(message.name)">
                            <div class="teal-text">{{message.name}}</div>
                            <div class="grey-text text-darken-3">{{message.content}}</div>
                            <div class="grey-text time">{{message.timestamp}}</div>
                        </div>
                    </li>
                </ul>
            </div>
            <div class="card-action">
                <NewMessage :name="name"/>
            </div>
        </div>
    </div>
</template>

<script>
import NewMessage from '@/components/NewMessage'
import db from '@/firebase/init'
import moment from 'moment'

export default {
    name: 'Chat',
    props: ['name'],
    components: {
        NewMessage
    },
    data() {
        return {
            messages: [],
        }
    },
    methods:{
        iam(messageName){
            if(messageName == this.name){
                return true
            }else{
                return false
            }
        }
    },
    created(){
        let ref = db.collection('messages').orderBy('timestamp')
        ref.onSnapshot((snapshot) => snapshot.docChanges().forEach(change => {
            if(change.type == 'added'){
                let doc = change.doc
                this.messages.push({
                    id: doc.id,
                    name: doc.data().name,
                    content: doc.data().content,
                    timestamp: moment(doc.data().timestamp).format('lll')
                })
            }
        }))
    }
}
</script>

<style>
.chat h2{
    font-size: 2.6em;
    margin-bottom: 40px;
}

.chat .messages {
    font-size: 1.4em;
}

.chat .time {
    display: block;
    font-size: 0.5em;
}
.messages {
    max-height: 300px;
    overflow: auto;
}
.chat .iam{
    padding-right: 10px;
}


</style>