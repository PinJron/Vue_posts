<template>
  <select 
    class="select"
    :value="modelValue" 
    @change="changeOptions">
      <option disabled value="">Make your choise</option>
      <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
      >
        {{ option.name }}
      </option>
  </select>
</template>
  
<script>
export default {
  name: 'my-select',
  props: {
    modelValue: {
      type: String
    },
    options: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    changeOptions(event) {
      console.log(event)
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>
.select {
  margin-top: 10px;
  border-radius: 6px;
  border: 2px solid rebeccapurple;
  padding: 10px 15px;
  border-radius: 6px;
}
</style>