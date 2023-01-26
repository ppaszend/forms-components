<template>
  <form v-if="form" @submit.prevent>
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
            :editable="formStore.editable"
            @addNewField="
              openAddNewFieldModal({
                stepId: step.id,
                sectionId: section.id,
                rowId: row.id,
                fieldId: field.id + 1,
              })
            "
            @editField="
              openEditFieldModal({
                stepId: step.id,
                sectionId: section.id,
                rowId: row.id,
                fieldId: field.id,
              })
            "
          >
            <component
              v-bind:is="fields[field.component]"
              v-bind="{
                ...field.props,
                ...fieldStylesCreator(field),
              }"
              v-model="
                formStore.formData.find(({ name }) => field.props.name === name)
                  .value
              "
            />
          </form-column>
        </form-row>
      </form-section>
    </form-step>
  </form>
  <add-new-field-modal />
  <modal-with-overlay v-model:visibility="editFieldModal"> </modal-with-overlay>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { useFormStore } from "@/stores/form";
import FormColumn from "@/components/shared/form/FormColumn.vue";
import FormSection from "@/components/shared/form/FormSection.vue";
import FormStep from "@/components/shared/form/FormStep.vue";
import FormRow from "@/components/shared/form/FormRow.vue";
import TextField from "@/components/shared/form/fields/TextField.vue";
import DateField from "@/components/shared/form/fields/DateField.vue";
import ModalWithOverlay from "@/components/modals/modalWithOverlay.vue";
import AddNewFieldModal from "@/components/modals/addNewFieldModal.vue";

import type { FieldExactPosition } from "@/shared/models/FieldExactPosition";
import type { Form } from "@/shared/models/Form";
import type { Field } from "@/shared/models/Field";

const props = defineProps<{ form: Form }>();

const formStore = useFormStore();

const fields = {
  TextField,
  DateField,
};

const fieldStylesCreator = (field: Field) => ({
  ...{ ...formStore.getThemeForField(field.component), ...field.styles },
});

const addNewFieldModal = ref(false);
const editFieldModal = ref(false);
const target = ref<FieldExactPosition | null>(null);

const openAddNewFieldModal = (newTarget: FieldExactPosition) => {
  target.value = newTarget;
  addNewFieldModal.value = true;
};

const openEditFieldModal = (newTarget: FieldExactPosition) => {
  target.value = newTarget;
  editFieldModal.value = true;
};
</script>
<script lang="ts">
export default {
  name: "FormComponent",
};
</script>

<style scoped></style>
