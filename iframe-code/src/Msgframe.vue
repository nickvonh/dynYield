<template>
    <iframe class="msgFrame" :data-id="user.id">
        <div>
            <input type="text" v-model="currentMessage" />
        </div>
    </iframe>
</template>

<script>
export default {
    name: 'message-frame',
    props: ['user'],
    data(){
        return {
            currentMessage : null
        }
    }
}
</script>

<style lang="stylus">
</style>
