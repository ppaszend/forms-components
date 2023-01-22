<template>
  <text-field
    :label="$props.label"
    v-model="value"
    @focus="opened = true"
    @blur="hideCalendar"
    :readonly="true"
    :name="$props.name"
  >
    <div class="date-picker">
      <date-picker v-show="opened" v-model="value" />
    </div>
  </text-field>
</template>

<script lang="ts" setup>
import { DatePicker } from "v-calendar";
import { ref } from "vue";
import TextField from "@/components/shared/form/fields/TextField.vue";
import props from "@/helpers/shared/props";

const $props = defineProps({
  name: props.name(),
  label: props.label(),
});

const opened = ref(false);
const value = ref(null);

const hideCalendar = () => {
  window.setTimeout(() => (opened.value = false), 100);
};
</script>
<script lang="ts">
export default {
  name: "DateField",
};
</script>

<style lang="scss" scoped>
.date-picker {
  position: absolute;
  top: calc(100% + 2px);
  left: 0;
}
</style>
