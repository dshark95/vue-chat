<template>
    <div id="app">
    <v-app id="inspire">              
            <div class="mx-auto">
                <v-text-field
                v-model="username"
                type="text"
                label="username"
                ></v-text-field>
            </div>
        <div>        
            <v-card
                elevation="16"
                max-width="400"
                class="mx-auto"
            >
                <v-virtual-scroll
                :bench="benched"
                :items="messages"
                height="300"
                item-height="64"
                >
                <template v-slot:default="{ item }">
                    <v-list-item :key="messages.indexOf(item)">
                    <!-- <v-list-item-action>
                        <v-btn
                        fab
                        small
                        depressed
                        color="primary"
                        >
                        {{ item.username }}
                        </v-btn>
                    </v-list-item-action> -->
        
                    <v-list-item-content>
                        <v-list-item-title>{{ item.username }}:</v-list-item-title>
                        <v-list-item-title>{{ item.message }}</v-list-item-title>                        
                    </v-list-item-content>
        
                    <!-- <v-list-item-action>
                        <v-icon small>
                        mdi-open-in-new
                        </v-icon>
                    </v-list-item-action> -->
                    </v-list-item>
        
                    <v-divider></v-divider>
                </template>              
            </v-virtual-scroll>                               
            </v-card>
                <v-responsive
                max-width="400"
                class="mx-auto mb-4">
                <form @submit.prevent="submit">
                    <v-text-field
                    v-model="message"
                    type="text"
                    label="message"
                    ></v-text-field>
                </form>                
                </v-responsive>            
        </div>
    </v-app>
    </div>
</template>
<script>
import { ref, onMounted } from 'vue';
import Pusher from 'pusher-js';

    export default {
        name: 'LazyChat',
        setup(){
            const messages = ref([{"username":"user 1", "message":"test"}]);
            const username = ref( "annoymouse");
            const message = ref("");

            onMounted(()=>{
                Pusher.logToConsole = true;

                var pusher = new Pusher('c8d02bfa11630368670d', {
                cluster: 'ap1'
                });

                var channel = pusher.subscribe('chat-app');
                channel.bind('message', (data)=> {
                    messages.value.push(data);
                });
            });

            const submit = async()=>{
                await fetch("http://localhost:8000/api/messages",{
                    method:"POST",
                    headers: {"Content-Type": "application/json"},
                    body:JSON.stringify({
                        username: username.value,
                        message: message.value
                    })
                })
                message.value = "";
            };

            return {
                username,
                message,
                messages,
                submit
            }
        
        },        
        data: () => ({
            "title":"test",
            benched: 0            
            
        }),
        computed: {
            items () {
                return Array.from({ length: this.length }, (k, v) => v + 1)
            },
            length () {
                return 7000
            },
        },
    }
</script>