<template>
  <q-btn
    rounded
    :icon="icon"
    :color="color"
    :label="label"
    :text-color="textColor"
    :size="size"
  />
</template>

<script>
export default {
  props: {
    color: String,
    label: String,
    size: String,
    textColor: String,
    icon: String,
  },
};
</script>

<style></style>
