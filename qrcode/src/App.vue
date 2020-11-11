<template>
  <div id="app">
    <BContainer class="pt-5">
      <h1 class="title text-center mb-4">QR Code Generator</h1>
      <Form
        :options="options"
        :isFormValid="isFormValid"
        @onShareOptions="updateOptions"
      />
      <QrCode
        :options="options"
        :isFormValid="isFormValid"
        :fullUrl="fullUrl"
      />
      <BDropdown :text="`History (${cachedCodes.length})`" class="qr-history">
        <p v-if="!cachedCodes.length" class="no-history">
          No history items.
        </p>
        <BListGroup v-else>
          <BListGroupItem
            v-for="(code, index) in cachedCodes"
            :key="index"
            class="d-flex justify-content-between align-items-center"
          >
            <BLink :href="code.fullUrl" target="_blank">{{ code.url }}</BLink>
            <div
              class="badges d-flex justify-content-between align-items-center"
            >
              <BBadge pill>{{ code.size }}</BBadge>
              <BBadge pill>
                <BIconEye v-if="code.isTransparent" />
                <BIconEyeFill v-else />
              </BBadge>
              <BBadge pill>{{ code.selectedExt }}</BBadge>
            </div>
          </BListGroupItem>
        </BListGroup>
      </BDropdown>
    </BContainer>
  </div>
</template>

<script>
import Form from "./components/Form.vue";
import QrCode from "./components/QrCode.vue";

export default {
  name: "App",
  data: () => ({
    cachedCodes: JSON.parse(localStorage.getItem("cachedCodes")) || [],
    options: {
      url: "",
      size: 5,
      isTransparent: false,
      selectedExt: "png",
    },
  }),
  components: {
    Form,
    QrCode,
  },
  computed: {
    isFormValid() {
      //eslint-disable-next-line
      const regexp = /^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\(\)\*\+,;=.]+$/gm;

      return Boolean(this.options.url.length && this.options.url.match(regexp));
    },
    fullUrl() {
      return `https://qrtag.net/api/qr${
        this.options.isTransparent ? "_transparent" : ""
      }_${this.options.size}.${this.options.selectedExt}?url=${
        this.options.url
      }`;
    },
  },
  methods: {
    makeToast(variant, title, msg) {
      this.$bvToast.toast(msg, {
        title: title,
        variant: variant,
        solid: true,
      });
    },
    updateOptions(obj) {
      this.options = { ...this.options, ...obj };

      if (!this.isFormValid) {
        this.makeToast(
          "danger",
          "Error",
          "Not enough data for creating QR code."
        );
        return;
      }

      this.addCode();
    },
    addCode() {
      const strCodes = JSON.stringify(this.cachedCodes);
      const strOptions = JSON.stringify({ ...this.options });
      const isCodeExists = strCodes.indexOf(strOptions) !== -1;

      if (!isCodeExists) {
        this.cachedCodes.push({ ...this.options, fullUrl: this.fullUrl });
        localStorage.setItem("cachedCodes", JSON.stringify(this.cachedCodes));
      }
    },
  },
};
</script>

<style lang="scss">
html,
body,
#app {
  height: 100%;
}

#app {
  display: flex;
  // align-items: center;
  justify-content: center;
  background: #444;
  color: #fff;
  overflow: auto;
}

.title {
  font-size: 32px;
}

.qr-history {
  position: absolute;
  left: 10px;
  top: 10px;

  .no-history {
    padding: 0 10px;
  }
}

.badges {
  margin-left: 5px;

  .badge {
    margin-left: 5px;
  }
}
.dropdown-menu.show {
  top: 10px !important;
  max-height: 88vh;
  overflow: auto;
}
</style>
