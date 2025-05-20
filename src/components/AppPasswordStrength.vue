<template>
  <base-message
    :labelMessage="safetyValues[safetyLevel].label"
    :class="safetyValues[safetyLevel].class"
  ></base-message>
</template>

<script>
let charPool = 0;
let entropy = 0;
const ALPHABET_LENGTH = 26
const SYMBOL_AMOUNT = 32
const DIGITS = 10
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
      },
    },
  },
  data() {
    return {
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
        { max: 850, label: "maximal sicher", class: "text-[#04BEC5]" },
      ], //max entropy is 838,98737
    };
  },
  methods: {
    entropyCalculationProcess() {
      this.resetVariables();
      this.updateCharacterPoolCount();
      this.calculateEntropy();
      this.setSafetyLevel();
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
          charPool += ALPHABET_LENGTH;
        } else if (this.labelArray[i] == "Kleinbuchstaben") {
          charPool += ALPHABET_LENGTH;
        } else if (this.labelArray[i] == "Symbole") {
          charPool += SYMBOL_AMOUNT;
        } else if (this.labelArray[i] == "Zahlen") {
          charPool += DIGITS;
        }
      }
    },
  },
};
</script>
