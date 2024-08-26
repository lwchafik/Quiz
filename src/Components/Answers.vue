<script setup>
import { computed } from 'vue';

  const props = defineProps({
    id: String,
    disabled: Boolean,
    value: String,
    correctAnswer: String
  })
  const model = defineModel()

  const classes = computed(() => ({
    disabled: props.disabled,
    right: props.disabled && props.value === props.correctAnswer,
    wrong: props.disabled && props.value !== props.correctAnswer && model.value === props.value
  }))

  const emits = defineEmits(['change'])
  const onChange = (event) => {
    emits('change', event)
  }
</script>

<template>
  <label :for="id" :class="classes">
    <input
      :disabled="disabled" :id="id"
      type="radio"
      name="answer"
      v-model="model"
      :value="value"
      @change="onChange"
    />
    {{ value }}
  </label>
</template>

<style>
.disabled {
  opacity: .5;
}
 .right {
  opacity: 1;
  color: green
 }
 .wrong {
  opacity: 1;
  color: red
 }
</style>