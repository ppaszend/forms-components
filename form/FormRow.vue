<template>
  <div class="row">
    <slot />
  </div>
</template>

<script>
export default {
  name: "FormRow",
};
</script>

<style lang="scss" scoped>
.row {
  display: flex;
}
</style>
