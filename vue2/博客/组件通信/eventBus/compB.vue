<template>
  <div class="containerB">
    <h3>this is CompB</h3>
    <input type="text" v-model="message" @keyup="sendMessage" />
    <p v-show="messageFromBus && sender !== $options.name">
      收到{{ sender }}的消息：{{ messageFromBus }}
    </p>
    <CompC />
    <CompD />
  </div>
</template>

<script>
import CompC from './compC'
import CompD from './compD'
export default {
  name: 'CompB',
  components: {
    CompC,
    CompD,
  },
  data() {
    return {
      message: '',
      messageFromBus: '',
      sender: '',
    }
  },
  mounted() {
    this.$bus.$on('sendMessage', (obj) => {
      const { sender, message } = obj
      this.sender = sender
      this.messageFromBus = message
    })
  },
  methods: {
    sendMessage() {
      this.$bus.$emit('sendMessage', {
        sender: this.$options.name,
        message: this.message,
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.containerB {
  border: 1px solid rgb(100, 217, 247);
  padding: 10px;
  margin-top: 100px;
}
</style>
