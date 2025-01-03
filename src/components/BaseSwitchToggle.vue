<template>
  <label class="flex cursor-pointer select-none items-center my-1">
    <div class="relative">
      <input type="checkbox" class="sr-only" @change="handleSwitchChange()" />
      <div class="block h-6 w-10 rounded-full bg-[#CAF0F8]"></div>
      <div
        :class="{ 'translate-x-full !bg-primary': checkedStatus }"
        class="dot absolute left-1 top-1 h-4 w-4 rounded-full bg-[#00B4D8] transition"
      ></div>
    </div>
    <p class="pl-2">{{ label }}</p>
  </label>
</template>
<script>
export default {
  name: "BaseSwitchToggle",
  props: {
    label: String,
  },
  data() {
    return {
      checkedStatus: true,
    };
  },
  beforeMount() {
    this.$emit("got_checked", this.label);
  },
  methods: {
    handleSwitchChange() {
      this.checkedStatus = !this.checkedStatus;
      this.emit_change();
    },
    emit_change() {
      this.$emit("got_checked", this.label);
    },
  },
};
</script>
