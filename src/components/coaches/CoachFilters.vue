<template>
  <div class="w-100 row justify-content-between my-2">
    <coach-filter-by-field
      @set-field="setField"
      :fields="fields"
    ></coach-filter-by-field>
    <coach-filter-by-search
      :search="search"
      @set-search="setSearch"
    ></coach-filter-by-search>
    <div class="mt-2"></div>
    <coach-filter-by-tags
      @set-tags="setTags"
      :tags="availableTags"
    ></coach-filter-by-tags>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from 'vue';
import { useStore } from 'vuex';
import CoachFilterByField from './CoachFilterByField.vue';
import CoachFilterBySearch from './CoachFilterBySearch.vue';
import CoachFilterByTags from './CoachFilterByTags.vue';
export default {
  components: {
    CoachFilterByField,
    CoachFilterBySearch,
    CoachFilterByTags,
  },

  setup() {
    const state = reactive({
      search: '',
      tags: [],
      field: '',
    });

    const store = useStore();

    const availableTags = computed(() => {
      return store.getters['coach/availableTags'];
    });

    const searchString = computed(() => {
      return store.getters['coach/searchString'];
    });

    const fields = computed(() => {
      return store.getters['coach/fields'];
    });

    function setSearch(value) {
      state.search = value;
      store.dispatch('coach/filterCoaches', state);
    }

    function setField(value) {
      state.field = value;
      store.dispatch('coach/filterCoaches', state);
    }

    function setTags(value) {
      state.tags = value;
      store.dispatch('coach/filterCoaches', state);
    }

    return {
      ...toRefs(state),
      availableTags,
      searchString,
      fields,
      setSearch,
      setField,
      setTags,
    };
  },
};
</script>
