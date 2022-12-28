<template>
  <div class="col-4">
    <input
      class="form-control"
      list="datalistOptions"
      id="exampleDataList"
      @keyup="onKeyUp"
      placeholder="Type to search..."
    />
    <!-- <datalist id="datalistOptions">
      <option value="San Francisco"></option>
      <option value="New York"></option>
      <option value="Seattle"></option>
      <option value="Los Angeles"></option>
      <option value="Chicago"></option>
    </datalist> -->
  </div>
</template>


<script>
import { useStore } from 'vuex';
export default {
  emits: ['setSearch'],
  setup(_, { emit }) {
    const store = useStore();

    function onKeyUp(e) {
      emit('setSearch', e.target.value);
    }

    function filterCoaches() {
      store.dispatch('coach/filterCoaches');
    }

    return {
      onKeyUp,
      filterCoaches,
    };
  },
};
</script>