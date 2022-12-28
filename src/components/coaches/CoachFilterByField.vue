<template>
  <div class="col-2">
    <select
      class="form-select"
      aria-label="Default select example"
      @click="onChange"
    >
      <option selected value="">Field</option>

      <option v-for="item in fields" :key="item" :value="item">
        {{ item }}
      </option>
    </select>
  </div>
</template>


<script>
import { toRefs } from 'vue';

export default {
  props: ['fields'],
  emits: ['setField'],
  setup(props, { emit }) {
    function onChange(e) {
      emit('setField', e.target.value);
    }

    return {
      ...toRefs(props),
      onChange,
    };
  },
};
</script>