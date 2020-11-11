<template>
  <BContainer>
    <BRow>
      <BCol sm="6" class="mx-auto">
        <BFormGroup>
          <BFormInput
            :value="localOptions.url"
            id="url"
            placeholder="Enter your URL"
            @input="updateProperty('url', $event)"
          />
        </BFormGroup>
      </BCol>
    </BRow>
    <BRow>
      <BCol sm="6" class="mx-auto text-center mb-2">
        <BFormGroup>
          <BFormSelect
            :value="options.size"
            @input="updateProperty('size', $event)"
          >
            <template v-for="num in 60">
              <BFormSelectOption v-if="num >= 2" :key="num" :value="num">
                {{ num }}
              </BFormSelectOption>
            </template>
          </BFormSelect>
        </BFormGroup>
        <BFormGroup>
          <BFormCheckbox
            :checked="options.isTransparent"
            switch
            size="lg"
            class="toggler"
            @input="updateProperty('isTransparent', $event)"
          >
            Transparent background
          </BFormCheckbox>
        </BFormGroup>
        <BFormGroup class="mb-4">
          <BFormRadioGroup
            :checked="options.selectedExt"
            :options="radioOptions"
            name="radio-inline"
            buttons
            @input="updateProperty('selectedExt', $event)"
          />
        </BFormGroup>
        <BFormGroup>
          <BButton variant="info" size="lg" @click="shareOptions">
            Generate
          </BButton>
        </BFormGroup>
      </BCol>
    </BRow>
  </BContainer>
</template>

<script>
export default {
  name: "Form",
  data: () => ({
    radioOptions: [
      { text: "PNG", value: "png" },
      { text: "SVG", value: "svg" },
    ],
    localOptions: {},
  }),
  props: {
    options: {
      type: Object,
      required: () => {},
    },
    isFormValid: {
      typs: Boolean,
      required: true,
    },
  },
  methods: {
    shareOptions() {
      this.$emit("onShareOptions", this.localOptions);
    },
    updateProperty(key, event) {
      console.log(event);
      this.localOptions[key] = event;
    },
  },
};
</script>

<style scoped lang="scss">
.toggler::v-deep {
  .custom-control-label {
    font-size: 16px;
    line-height: 2;
  }
}
</style>
