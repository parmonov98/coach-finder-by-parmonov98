<template>
  <base-layout>
    <base-modal :show="!!error">
      <p>{{ error }}</p>
    </base-modal>
    <div v-if="!isLoading">
      <h2>Requests</h2>
      <div class="row mb-2">
        <div class="col-12">
          <input
            class="form-control"
            type="search"
            v-model="search"
            @keyup="searchRequests"
            placeholder="Searching request by name and subject"
          />
        </div>
      </div>
      <requests-list></requests-list>
    </div>
    <base-loading-panel v-if="isLoading"></base-loading-panel>
  </base-layout>
</template>

<script>
import { ref } from '@vue/reactivity';
import { useStore } from 'vuex';
import RequestsList from '../../components/requests/RequestsList.vue';
export default {
  components: {
    RequestsList,
  },
  setup() {
    const store = useStore();
    const isLoading = ref(true);
    const error = ref();
    async function loadRequests() {
      const responseData = await store.dispatch('requests/getRequests');
      console.log(responseData);
      if (responseData !== true) {
        error.value = responseData.error;
      }
      isLoading.value = false;
    }

    loadRequests();

    function searchRequests(e) {
      store.dispatch('requests/filterRequests', e.target.value);
    }

    return {
      isLoading,
      error,
      loadRequests,
      searchRequests,
    };
  },
};
</script>