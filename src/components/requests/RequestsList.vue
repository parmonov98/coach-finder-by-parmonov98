<template>
  <div class="row" v-if="hasRequests">
    <request-item
      v-for="item in requests"
      :key="item.id"
      :name="item.name"
      :title="item.title"
      :description="item.description"
      :email="item.email"
    ></request-item>
  </div>
  <div v-else class="no-item-found">No requests found</div>
</template>

<script>
import { computed } from 'vue';
import { useStore } from 'vuex';
import RequestItem from './RequestItem.vue';

export default {
  components: {
    RequestItem,
  },
  setup() {
    const store = useStore();
    const hasRequests = computed(() => store.getters['requests/hasRequests']);
    const requests = computed(() => store.getters['requests/requests']);

    return {
      hasRequests,
      requests,
    };
  },
};
</script>