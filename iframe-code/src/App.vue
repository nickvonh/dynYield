<template>
    <div class="frame">
        <ul class="messages">
            <li v-for="each in messages">
                <span class="user">{{each.user}}: </span> {{each.msg}}
            </li>
        </ul>
        <div class="input">
            <input type="text" v-model="currentMessage" @keyup.enter="sendMsg()"/>
            <button  @click="sendMsg()">send</button>
        </div>
    </div>
</template>

<script>
import Msgframe from './Msgframe.vue'
export default {
    name: 'app',
    components: {Msgframe},
    mounted(){
        window.parent.postMessage('userInit', '*')

        window.addEventListener('message', (event)=>{
            if(/parentMsg/.test(event.data)){
                let data = JSON.parse(event.data.split(';')[1])
                this.messages.push(data)
            }
        })
    },
    data () {
        return {
            messages: [],
            currentMessage : null
        }
    },
    methods: {
        sendMsg(){
            window.parent.postMessage('userMsg;' + this.currentMessage, '*')
            this.currentMessage = '';
        }
    }
}
</script>

<style lang="stylus" scoped>
@import './stylus/resources.styl'
.frame
    background rgba(180,190,210,1)
    font-family sans-serif
    font-smoothing antialiased
    width 400px
    height 200px
    padding 20px
    grid('column','','')
    justify-content(space-between)
    .messages
        list-style none
        height 80%
        color white
        overflow-y scroll
        .user
            color gray
    .input
        height 20%
        input
            height 100%
            width 80%
            border-radius 5px
            border none
            padding 5px
        button
            padding 5px 20px
            height 100%
            border none
            background white
            border-radius 5px
            cursor pointer
</style>
