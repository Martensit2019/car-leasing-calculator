<template>
  <div class="form__input">
    <label for="total-cost" class="label">{{ label }}</label>
    <div>
      <input
        :type="isReadonly ? 'text' : 'number'"
        :value="isReadonly ? percentValue : modelValue"
        @input="updateInput"
        @change="updateChange"
        class="input"
        :class="{ 'not-focus': isReadonly }"
        :readonly="isReadonly"
      />
      <div v-if="isReadonly" class="input-percent__wrapper">
        <input :value="modelValue" type="number" class="input-percent" @input="updateInput" @change="updateChange" />
        <span>%</span>
      </div>
      <div v-else class="params-text">{{ text }}</div>
    </div>
    <div class="input-range__wrapper">
      <input
        type="range"
        :min="min"
        :max="max"
        :value="modelValue"
        class="input-range"
        :style="{
          background: `-webkit-linear-gradient(left, #ff9514 0%, #ff9514 ${rangeValue}%, #E5E5E5 ${rangeValue}%, #E5E5E5 100%)`,
        }"
        @input="updateInput"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'input-app',
  props: {
    modelValue: {
      type: Number,
      default: 0,
    },
    label: {
      type: String,
      default: '',
    },
    isReadonly: {
      type: Boolean,
      default: false,
    },
    text: {
      type: String,
      default: '',
    },
    min: {
      type: Number,
      default: 0,
    },
    max: {
      type: Number,
      default: 0,
    },
    total: {
      type: Number || String,
      default: 1,
    },
  },
  methods: {
    updateInput(e) {
      this.$emit('update:modelValue', Number(e.target.value))
    },
    updateChange(e) {
      if (Number(e.target.value) < this.min) this.$emit('update:modelValue', this.min)
      if (Number(e.target.value) > this.max) this.$emit('update:modelValue', this.max)
    },
  },
  computed: {
    percentValue() {
      return Math.round((this.modelValue * this.total) / 100) + '₽'
    },
    rangeValue() {
      return ((this.modelValue - this.min) / (this.max - this.min)) * 100
    },
  },
}
</script>
