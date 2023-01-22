<template>
  <field-wrapper :label="$props.label" :for="$props.name">
    <input
      :id="$props.name"
      class="input"
      :type="$props.type"
      v-model="value"
      @focus="$emit('focus')"
      @blur="$emit('blur')"
      :disabled="disabled"
      :readonly="readonly"
    />
    <slot />
  </field-wrapper>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
import FieldWrapper from "@/components/form/fields/FieldWrapper.vue";
import props from "@/helpers/props";

const $emit = defineEmits(["update:modelValue", "focus", "blur"]);

const $props = defineProps({
  name: props.name(),
  modelValue: {
    type: String,
    default: "",
    required: true,
  },
  type: {
    type: String,
    default: "text",
    validator: (type: string) =>
      ["text", "number", "email", "phone"].includes(type),
  },
  label: props.label(),
  inputHeight: props.cssUnit("40px"),
  backgroundColor: props.hexColor("#fff"),
  borderColor: props.hexColor("#000"),
  borderRadius: props.cssUnit("0px"),
  fontSize: props.cssUnit("1em"),
  disabled: {
    type: Boolean,
    default: false,
  },
  readonly: {
    type: Boolean,
    default: false,
  },
});

const value = ref($props.modelValue);

watch(value, (newValue) => {
  $emit("update:modelValue", newValue);
});

watch(
  () => $props.modelValue,
  (newValue) => {
    value.value = newValue;
  }
);
</script>
<script lang="ts">
export default {
  name: "TextField",
};
</script>

<style lang="scss" scoped>
.input {
  outline: none;

  background-color: v-bind("backgroundColor");
  border: 1px solid v-bind("borderColor");
  border-radius: v-bind("borderRadius");
  font-size: v-bind("fontSize");
  height: v-bind("inputHeight");
}
</style>
