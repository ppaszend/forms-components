<template>
  <form v-if="form">
    <form-step v-for="step in form.steps" :key="step.id">
      <form-section
        v-for="section in step.sections"
        :key="section.id"
        v-bind="{ ...section.props, ...form.theme.section }"
      >
        <form-row v-for="row in section.rows" :key="row.id">
          <form-column
            v-for="field in row.fields"
            :key="field.id"
            :cols="field.cols"
          >
            <component
              v-bind:is="fields[field.component]"
              v-bind="{
                ...field.props,
                ...fieldStylesCreator(field),
              }"
              v-model="
                formData.find(({ name }) => field.props.name === name).value
              "
            />
          </form-column>
        </form-row>
      </form-section>
    </form-step>
  </form>
</template>

<script lang="ts" setup>
import FormColumn from "@/components/form/FormColumn.vue";
import FormSection from "@/components/form/FormSection.vue";
import FormStep from "@/components/form/FormStep.vue";
import FormRow from "@/components/form/FormRow.vue";
import { ref } from "vue";
import { FieldData } from "@/models/FieldData";
import TextField from "@/components/form/fields/TextField.vue";
import DateField from "@/components/form/fields/DateField.vue";
import type { Field, Form } from "@/models/Form";

const props = defineProps<{ form: Form }>();

const fields = {
  TextField,
  DateField,
};

const formData = ref<FieldData[]>([]);
props.form.steps.forEach((step) => {
  step.sections.forEach((section) => {
    section.rows.forEach((row) => {
      formData.value.push(
        ...row.fields.map((field) => ({
          name: field.props.name,
          value: "",
        }))
      );
    });
  });
});

const getThemeForField = (fieldComponentName: string): object => {
  const fieldTheme = props.form.theme.fields.find(
    ({ component }) => fieldComponentName === component
  );
  return fieldTheme?.styles || {};
};

const fieldStylesCreator = (field: Field) => ({
  ...{ ...getThemeForField(field.component), ...field.styles },
});
</script>
<script lang="ts">
export default {
  name: "FormComponent",
};
</script>

<style scoped></style>
