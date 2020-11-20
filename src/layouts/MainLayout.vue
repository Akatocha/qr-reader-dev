<template>
  <q-layout class="row justify-center">
    <div class="my-box column">
      <h6 class="decode-result my-h6">Last result: <b id="result">{{ result }}</b></h6>
      <div class="row justify-between content-between">
        <q-btn 
          class="q-mb-md"
          @click="copy"
          :disable='trueCoppy'           
          icon-right="content_paste" 
          label="Copy" />
        <q-btn
          class="q-mb-md"
          target="_blank"
          :href="result"
          type="a"
          :disable='trueCoppy'
          icon-right="content_copy" 
          label="Open in new window" />
        </div>
      <div class="screen self-center">
        <p 
        v-if="trueError"
        class="error screen-error">{{ error }}</p>
      <qrcode-stream         
        @decode="onDecode" 
        @init="onInit" />
        </div>
        <div class="icon-box self-center q-mt-md">
          <q-icon
          size="xl"
          name="touch_app"
        ></q-icon>
        </div>        
        <p
        class=" text-center text-bold text-subtitle1"
        >Double touch to make a screen</p>
    </div>
  </q-layout>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'


export default {

  components: { QrcodeStream },

  data () {
    return {
      result: '',
      error: ''
    }
  },

  methods: {
    copy(){
    let copy = document.getElementById("result")
    let copyText = copy.innerText
    navigator.clipboard.writeText(copyText)    
  },
    onDecode (result) {
      this.result = result
    },
    async onInit (promise) {
      try {
        await promise
      } catch (error) {
        if (error.name === 'NotAllowedError') {
          this.error = "ERROR: you need to grant camera access permisson"
        } else if (error.name === 'NotFoundError') {
          this.error = "ERROR: no camera on this device"
        } else if (error.name === 'NotSupportedError') {
          this.error = "ERROR: secure context required (HTTPS, localhost)"
        } else if (error.name === 'NotReadableError') {
          this.error = "ERROR: is the camera already in use?"
        } else if (error.name === 'OverconstrainedError') {
          this.error = "ERROR: installed cameras are not suitable"
        } else if (error.name === 'StreamApiNotSupportedError') {
          this.error = "ERROR: Stream API is not supported in this browser"
        }
      }
    }
  },
  computed: {
    trueCoppy() {
      if (this.result == ''){
        return true
      }else{
        return false
      }
    },
    trueError(){
      if(this.error == ''){
        return false
      }else{
        return true
      }
    }
  },
}
</script>

<style>
.icon-box{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  border:2px solid #45a247;
  background: linear-gradient(to top, #283c86, #45a247);
}
.q-btn{
  background: #45a247;
  color: white;
}
.q-btn.disabled {
  /* opacity: 1 !important; */
}
.my-box{
  width: 100%;
  max-width: 900px;
  padding: 10px;
}
.my-h6{
  border-radius: 5px;
  padding: 0 10px;
  background: #3e3e3ec9;
  color: white;
  margin: 10px 0 15px 0;
}
.screen{
  position: relative;
  background: #60df62;
  width: 80vw;
  max-width: 600px;
  height: auto;
}
.screen-error{
  position: absolute;
  top: 40%;
  left: 5%;
}
.error {
  font-weight: bold;
  color: #ca0a0ae3;
}
</style>