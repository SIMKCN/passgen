<template>
  <div
    class="grid grid-cols-1 mt-5 mb-5 rounded-lg w-72 bg-[#0077B6] text-[#CAF0F8] px-2 py-2"
  >
    <base-switch-toggle
      label="Großbuchstaben"
      @got_checked="updateCheckboxArrayAndSendToApp($event)"
    ></base-switch-toggle>
    <base-switch-toggle
      label="Kleinbuchstaben"
      @got_checked="updateCheckboxArrayAndSendToApp($event)"
    ></base-switch-toggle>
    <base-switch-toggle
      label="Symbole"
      @got_checked="updateCheckboxArrayAndSendToApp($event)"
    ></base-switch-toggle>
    <base-switch-toggle
      label="Zahlen"
      @got_checked="updateCheckboxArrayAndSendToApp($event)"
    ></base-switch-toggle>
  </div>
</template>
<script>
import BaseSwitchToggle from "./BaseSwitchToggle.vue";

export default {
  name: "characterSelector",
  data() {
    return {
      checkboxesLabels: [
        { label: "Großbuchstaben", checked: false },
        { label: "Kleinbuchstaben", checked: false },
        { label: "Symbole", checked: false },
        { label: "Zahlen", checked: false },
      ],
    };
  },
  components: {
    BaseSwitchToggle,
  },
  methods: {
    updateCheckboxArrayAndSendToApp($event) {
      this.updateCheckboxArray($event);
      const labels_checked_boxes = this.createArrayOfCheckedBoxes();
      this.emitLabelsOrErrorToApp(labels_checked_boxes);
    },

    updateCheckboxArray($event) {
      for (let i = 0; i < this.checkboxesLabels.length; i++) {
        if ($event == this.checkboxesLabels[i].label) {
          this.checkboxesLabels[i].checked = !this.checkboxesLabels[i].checked;
        }
      }
    },
    createArrayOfCheckedBoxes() {
      const checked_boxes = this.checkboxesLabels.filter(
        (item) => item.checked === true
      );
      return checked_boxes.map((item) => item.label);
    },
    emitLabelsOrErrorToApp(labels_checked_boxes) {
      if (labels_checked_boxes.length != 0) {
        this.$emit("update_label_array", labels_checked_boxes);
      } else {
        this.$emit("error_empty_label_array");
      }
    },
  },
};
</script>
