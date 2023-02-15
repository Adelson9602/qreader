<template>
  <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    >
    <div class="row justify-center q-pt-lg">
       <div class="col-12 text-center">
        <span class="text-subtitle2 text-grey-9">
          {{ textInfo }}
        </span>
        <q-btn color="blue-grey-10" rounded icon="camera_alt" label="Read QRCode"
          class="full-width" size="lg" @click="turnCameraOn()"
          v-show="!showCamera"/>

          <p class="text-subtitle1" v-if="result">Last result: <b>{{ result }}</b></p>
          <div v-if="showCamera">
            <qrcode-stream :camera="camera" @decode="onDecode"/>
          </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref, computed } from 'vue'
import { QrcodeStream } from 'qrcode-reader-vue3'


export default defineComponent({
  name: 'IndexPage',
  components: {
    QrcodeStream
  },
  setup () {
    const showCamera = ref(false)
    const camera = ref('')
    const result = ref('')

    const onDecode = (value) => {
      result.value = value
      turnCameraOff()
    }

    const turnCameraOn = () => {
      camera.value = 'auto'
      showCamera.value = true
    }

    const turnCameraOff = () => {
      camera.value = 'off'
      showCamera.value = false
    }

    const textInfo = computed({
      // getter
      get() {
        return showCamera.value ? 'position the qrcode on the camera' : 'Press the button and scan a qrcode.'
      }
  })

    return {
      onDecode,
      turnCameraOn,
      turnCameraOff,
      textInfo,
      showCamera,
      result,
      camera
    }
  }
})
</script>
