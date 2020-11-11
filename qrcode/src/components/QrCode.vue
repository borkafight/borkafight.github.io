<template>
  <BRow>
    <BCol sm="6" class="mx-auto text-center">
      <div class="qr-code">
        <span class="selected-ext">{{ options.selectedExt }}</span>
        <BLink v-if="isFormValid" :href="fullUrl" target="_blank">
          <BImg :src="fullUrl" fluid :alt="options.url" />
        </BLink>
        <span v-else class="no-qr-code">Your QR code here</span>
      </div>
    </BCol>
  </BRow>
</template>

<script>
export default {
  name: "QrCode",
  props: {
    options: {
      type: Object,
      required: true,
    },
    isFormValid: {
      type: Boolean,
      required: true,
    },
    fullUrl: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped lang="scss">
.qr-code {
  position: relative;
  background: #555;
  margin: 0 0 50px;
  padding: 20px;
  border-radius: 5px;
}

.no-qr-code {
  position: relative;
  padding: 10px;
  font-size: 20px;
  text-transform: uppercase;
}

.selected-ext {
  position: absolute;
  top: -20px;
  right: 0px;
  font-size: 12px;
  color: #999;
  text-transform: uppercase;
}
</style>
