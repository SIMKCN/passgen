<template>
  <div>
    <div class="grid grid-cols-1">
      <base-label
        :label-text="`Länge: ${passwordLength}`"
        class="rounded-lg pb-2"
      ></base-label>
      <div class="grid grid-cols-2 mt-3">
        <input
          type="range"
          value="12"
          min="4"
          max="128"
          class="mr-2 w-72 rounded-lg mb-2"
          v-model="passwordLength"
          @change="sendLengthToApp"
          step="1"
        />
      </div>
    </div>
  </div>
</template>
<script>
import BaseLabel from "./BaseLabel.vue";
export default {
  name: "AppLength",
  components: {
    BaseLabel,
  },
  data() {
    return {
      passwordLength: 12,
    };
  },
  beforeMount() {
    this.$emit("update_password_length", this.passwordLength);
  },
  methods: {
    sendLengthToApp() {
      this.$emit("update_password_length", this.passwordLength);
    },
  },
};
</script>
