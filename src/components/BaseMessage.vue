<template>
  <div>
    <p class="font-semibold text-sm" :class="textColor">
      {{ labelMessage }}
    </p>
  </div>
</template>

<script>
export default {
  name: "BaseMessage",
  props: {
    labelMessage: String,
    textColor: String,
  },
};
</script>
