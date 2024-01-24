<template>
  <qrcode-stream @detect="onDetect" :paused="paused" style="height:max-content;width:max-content">
      <div v-show="paused" style="width:100%; height:100%; background-color: rgba(255, 255, 255, 0.8);">
          <h2>QRコードを読み取りました!</h2>
          <pre>{{ scanedtext }}</pre>
          <h3>サーバに送信しますか？</H3>
          <input type="button" @click="onClick" value="送る"></input>
          <input type="button" @click="() => this.paused = false" value="キャンセル"></input>
      </div>
  </qrcode-stream>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'

export default {
  name: "App",
  components: {
    QrcodeStream
  },
  data: function(){
      return {
          scanedtext: "",
          paused: false
      }
  },
  methods: {
      onDetect: async function(data){
          this.scanedtext = data[0].rawValue;
          this.paused = true;
      },
      onClick: function(){
          const msg = {text: this.scanedtext};
          if(window.opener)
          {
            window.opener.postMessage(msg, '*');
          }
          window.parent.postMessage(msg, '*');
          this.paused = false;
      }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
</style>
