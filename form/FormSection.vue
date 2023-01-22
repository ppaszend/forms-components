<template>
  <div class="form-section">
    <div class="form-section__title" v-if="title">
      {{ title }}
    </div>
    <div class="form-section__body">
      <slot />
    </div>
  </div>
</template>

<script lang="ts" setup>
import props from "@/helpers/props";
import { cssAllSidesUnitValidator } from "@/helpers/propValidator";

const $props = defineProps({
  title: {
    type: String,
    required: false,
  },
  height: props.cssUnit("40px"),
  fontSize: props.cssUnit("20px"),
  fontWeight: {
    type: String,
    default: "400",
    validator: (value: string) =>
      ["300", "400", "500", "600", "700", "800", "900"].includes(value),
  },
  titleBackground: props.hexColor("#ffffff"),
  padding: {
    type: String,
    default: "0px",
    validator: cssAllSidesUnitValidator,
  },
  titlePadding: {
    type: String,
    default: "0px",
    validator: cssAllSidesUnitValidator,
  },
  borderRadius: {
    type: String,
    default: "0px",
    validator: cssAllSidesUnitValidator,
  },
});
</script>
<script lang="ts">
export default {
  name: "FormSection",
};
</script>

<style lang="scss" scoped>
.form-section {
  border-radius: v-bind("borderRadius");
  overflow: hidden;

  .form-section__title {
    display: flex;
    align-items: center;
    height: v-bind("height");
    font-size: v-bind("fontSize");
    font-weight: v-bind("fontWeight");
    background-color: v-bind("titleBackground");
    padding: v-bind("titlePadding");
  }

  .form-section__body {
    padding: v-bind("padding");
  }
}
</style>
