<template>
  <div
    class="box"
    :style="{
      ...$attrs.style,
      color,
      backgroundColor: '#0fc400',
      borderWidth,
    }"
  >
    <slot />
  </div>
</template>
<script>
export default {
  props: {
    color: {
      type: String,
      default: '#ccc',
    },
    borderWidth: {
      type: String,
      default: '1em',
    },
  },
};
</script>
<style lang="scss" scoped>
.box {
  border: solid currentColor;
  height: 100%;
  max-height: 100%;
  max-width: 100%;
}
</style>
