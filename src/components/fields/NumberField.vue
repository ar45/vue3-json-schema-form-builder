<script lang="ts">
import { defineComponent, PropType } from 'vue';

import { useDescription, useTitle } from './hooks';
import BaseWidget from '../widgets/BaseWidget.vue';
import { JsonSchema, UISchema, FieldProps } from '/@/types';

export default defineComponent({
  name: 'NumberField',
  components: { BaseWidget },
  props: {
    formData: { type: Number, default: undefined },
    jsonSchema: { type: Object as PropType<JsonSchema>, required: true },
    uiSchema: { type: Object as PropType<UISchema<number>>, required: true },
    fieldName: { type: String, required: true },
    required: { type: Boolean, required: true },
  },
  setup(props: FieldProps<number>, { emit }) {
    function onUpdate(value: string) {
      const newValue = value === '' ? undefined : +value;
      emit('update:formData', newValue);
      console.debug('NumberField: emit', newValue);
    }
    return {
      title: useTitle(props),
      description: useDescription(props),
      onUpdate,
    };
  },
});
</script>

<template>
  <div class="number-field">
    <label class="title" v-if="title" v-text="title" />
    <p class="description" v-if="description" v-text="description" />
    <BaseWidget
      :modelValue="formData"
      :jsonSchema="jsonSchema"
      :uiSchema="uiSchema"
      :required="required"
      @update:modelValue="onUpdate"
    />
  </div>
</template>