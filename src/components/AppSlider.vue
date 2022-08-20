<template>
  <div class="slider">
    <label :for="id" :title="hint">
      <slot>{{ label }}</slot>
    </label>
    <div class="slider-inner">
      <span v-if="startIcon">{{ startIcon }}</span>
      <input
        v-bind="$attrs"
        :id="id"
        :value="modelValue"
        type="range"
        @input="handleInput"
      />
      <span v-if="endIcon">{{ endIcon }}</span>
    </div>

    <output :for="id">{{ modelValue }}</output>
  </div>
</template>
<script>
export default {
  name: 'AppSlider',
  props: {
    id: {
      type: String,
      default: Math.floor(Math.random() * 100000).toString(),
    },
    label: {
      type: String,
      default: '',
    },
    hint: {
      type: String,
      default: '',
    },
    modelValue: {
      type: Number,
      required: true,
    },
    startIcon: {
      type: String,
      default: '',
    },
    endIcon: {
      type: String,
      default: '',
    },
  },
  emits: ['update:model-value'],
  setup(props, { emit }) {
    function handleInput(val) {
      emit('update:model-value', Number(val.target.value));
    }
    return {
      handleInput,
    };
  },
};
</script>
<style lang="scss">
.slider {
  display: flex;
  flex-direction: column;

  &-inner {
    display: flex;
    align-items: center;
  }

  label {
    font-size: 0.75em;
    color: #7a7b7a;
  }

  output {
    font-weight: 500;
    margin-top: 0.25em;
  }
}
</style>
