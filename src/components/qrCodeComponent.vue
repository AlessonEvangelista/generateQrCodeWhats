<template>
  <div>
    <qrcode-vue :value="value" :size="size" level="H"></qrcode-vue>
  </div>
</template>

<script>
import QrcodeVue from "qrcode.vue";
export default {
  data() {
    return {
      value: "https://example.com",
      size: 300
    };
  },
  components: {
    QrcodeVue
  }
};
</script>
