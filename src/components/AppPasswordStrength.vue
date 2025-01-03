<template>
  <base-message
    :labelMessage="safetyValues[safetyLevel].label"
    :class="safetyValues[safetyLevel].class"
  ></base-message>
</template>

<script>
let charPool = 0;
let entropy = 0;

import BaseMessage from "./BaseMessage.vue";
export default {
  name: "passwordStrength",
  components: {
    BaseMessage,
  },
  props: {
    password: String,
    labelArray: Array,
  },
  mounted() {
    this.entropyCalculationProcess();
  },
  watch: {
    password: {
      handler(password) {
        this.entropyCalculationProcess();
        console.log(password, password.length, this.labelArray, charPool);
      },
    },
  },
  data() {
    return {
      passwordStrength: "",
      safetyLevel: 0,
      safetyValues: [
        { max: 40, label: "sehr unsicher", class: "text-[#ff0000]" },
        { max: 50, label: "akzeptabel", class: "text-[#ff6300]" },
        { max: 60, label: "fast sicher", class: "text-[#ff6300]" },
        { max: 80, label: "sicher", class: "text-[#ff6300]" },
        { max: 100, label: "sehr sicher", class: "text-[#3DC000]" },
        { max: 120, label: "besonders sicher", class: "text-[#3DC000]" },
        { max: 160, label: "äußerst sicher", class: "text-[#34C53E]" },
        { max: 200, label: "höchst sicher", class: "text-[#34C53E]" },
        { max: 220, label: "absolut sicher", class: "text-[#1AC883]" },
        { max: 500, label: "enorm sicher", class: "text-[#1AC883]" },
        { max: 800, label: "maximal sicher", class: "text-[#04BEC5]" },
        { max: 1700, label: "ultimativ sicher", class: "text-[#04BEC5]" },
      ],
    };
  },
  methods: {
    entropyCalculationProcess() {
      this.resetVariables();
      this.updateCharacterPoolCount();
      this.calculateEntropy();
      this.setSafetyLevel();

      console.log(entropy, charPool);
      this.resetVariables();
    },
    resetVariables() {
      charPool = 0;
      entropy = 0;
    },

    calculateEntropy() {
      entropy = this.password.length * Math.log2(charPool);
    },
    setSafetyLevel() {
      this.safetyLevel = this.safetyValues.findIndex(
        ({ max }) => entropy <= max
      );
    },

    updateCharacterPoolCount() {
      for (let i = 0; i < this.labelArray.length; i++) {
        if (this.labelArray[i] == "Großbuchstaben") {
          charPool += 26;
        } else if (this.labelArray[i] == "Kleinbuchstaben") {
          charPool += 26;
        } else if (this.labelArray[i] == "Symbole") {
          charPool += 32;
        } else if (this.labelArray[i] == "Zahlen") {
          charPool += 10;
        }
      }
    },
  },
};
</script>
