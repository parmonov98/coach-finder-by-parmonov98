<template>
  <base-layout>
    <div class="row jusify-content-between">
      <div class="col-8">
        <h2 class="mr-auto">Coaches</h2>
      </div>
      <div class="col-4 text-end">
        <base-button
          v-if="!isAuthed"
          :to="{ name: 'sign-in', query: { redirect: 'couches-register' } }"
        >
          Register as a couch
        </base-button>

        <base-button v-else :to="{ name: 'requests' }"> Requests </base-button>
      </div>
    </div>
    <base-loading-panel v-if="isLoading"></base-loading-panel>
    <div class="d-flex justify-content-center">
      <coach-filters></coach-filters>
    </div>
    <div class="row mt-3">
      <coaches-list></coaches-list>
    </div>
  </base-layout>
</template>

<script>
import { computed, ref } from 'vue';
import { useStore } from 'vuex';
import CoachesList from '../../components/coaches/CoachesList.vue';
import CoachFilters from '../../components/coaches/CoachFilters.vue';
export default {
  components: {
    CoachesList,
    CoachFilters,
  },
  setup() {
    const store = useStore();
    const isLoading = ref(false);

    loadCouches();

    const isAuthed = computed(() => store.getters['auth/isAuthed']);

    async function loadCouches() {
      isLoading.value = true;
      await store.dispatch('coach/getCoaches');
      isLoading.value = false;
    }

    return { isLoading, isAuthed, loadCouches };
  },
};
</script>