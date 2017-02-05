<template>
  <div id="app">
      <div class="settings">
          <div class="button-box">
              <div class="button" @click="addUser()">
                  add
              </div>
          </div>
          <div class="button-box">
              <div class="button" @click="removeUser()">
                  remove
              </div>
          </div>
      </div>
      <div id="frames">
          <div v-for="each in users" class="userFrame">
              <h2>iframe{{each.id}}</h2>
              <iframe :srcdoc="iframe" :id="each.id" width="400px" height="200px"></iframe>
          </div>
      </div>
  </div>
</template>

<script>
import Msgframe from './Msgframe.vue'
import iframe from './iframe.html'
export default {
    name: 'app',
    components: {Msgframe},
    mounted(){
        window.addEventListener('message', (event)=>{
            if(typeof event.data === 'string'){
                let data,
                    originFrame,
                    frames = document.getElementsByTagName('iframe');

                for(let id = 0; id<frames.length; id++){
                    if(frames[id].contentWindow === event.source){
                        originFrame = id
                    }
                }

                data = event.data.split(';')
                if(!!data && data[0] === 'userMsg'){
                    let msg = {user:'iframe' + originFrame, msg:data[1]}
                    this.messages.push(msg)
                    this.sendMsg(msg)
                }

                if(data === 'userInit'){
                    console.log('first')
                    for(let each of messages){
                        frames[id].contentWindow.postMessage('parentMsg;'+JSON.stringify(each), "*")
                    }

                }
            }


        })
    },
    data () {
        return {
            iframe: iframe,
            messages: [],
            users: []
        }
    },
    methods: {
        addUser(){
            let newUser = {id : this.users.length + 1};
            let newFrame;

            this.users.push(newUser)
            this.messages.push({user:'system', msg: 'iframe' + newUser.id + ' has joined'})
            this.sendMsg({user:'system', msg: 'iframe' + newUser.id + ' has joined'})
        },
        removeUser(){
            this.users.splice(this.users.length-1,1)
        },
        sendMsg(msg){
            let frames = document.getElementsByTagName('iframe');

            for(let each of frames){
                each.contentWindow.postMessage('parentMsg;'+JSON.stringify(msg), '*')
            }
        }
    }
}
</script>

<style lang="stylus">
@import './stylus/resources.styl'

#app
    grid('column','center','start')
    font-family Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    width 900px
    margin 0 auto
    height 100vh
    .settings
        cell(1,5)
        width 100%
        grid('row','','center')
        .button-box
            cell(1,2)
            grid('row','center','center')
            .button
                height 50px
                width 50%
                border-radius 5px
                cursor pointer
                background gray
                color white
                text-align center
                line-height 50px
    #frames
        width 100%
        cell(4,5)
        grid('row','','start')
        flex-wrap(wrap)
        h2
            color blue
            font-weight 300
            font-size 1.2em
        .userFrame
            cell(1,2)
            iframe
                width 100%
                border none
</style>
