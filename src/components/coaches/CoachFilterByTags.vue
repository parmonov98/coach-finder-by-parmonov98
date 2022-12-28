<template>
  <div class="col-auto">
    <div class="form-check form-check-inline" v-for="item in tags" :key="item">
      <input
        class="form-check-input"
        v-model="selected"
        type="checkbox"
        name="tags"
        :id="item"
        @change="onChange"
        :value="item"
      />
      <label class="form-check-label" :for="item">{{ item }}</label>
    </div>
  </div>
</template>


<script>
import { ref } from '@vue/reactivity';
import { toRefs } from 'vue';
export default {
  props: ['tags'],
  emits: ['setTags'],
  setup(props, { emit }) {
    const selected = ref([]);
    function onChange() {
      emit('setTags', selected.value);
    }

    return {
      ...toRefs(props),
      selected,
      onChange,
    };
  },
};
</script>