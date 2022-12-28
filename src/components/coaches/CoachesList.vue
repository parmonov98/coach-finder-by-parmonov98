<template>
  <div class="row justify-content-center" v-if="hasCoaches">
    <coach-item
      v-for="item in coaches"
      :key="item.id"
      :id="item.userId"
      :first_name="item.first_name"
      :last_name="item.last_name"
      :photo="item.photo"
      :description="item.description"
      :tags="item.tags ?? []"
      :field="item.field"
      :email="item.email"
    ></coach-item>
  </div>
  <div v-else class="no-item-found">No coaches found</div>
</template>

<script>
import { computed } from 'vue';
import { useStore } from 'vuex';
import CoachItem from './CoachItem.vue';
export default {
  components: {
    CoachItem,
  },

  setup() {
    const store = useStore();

    const hasCoaches = computed(() => {
      return store.getters['coach/hasCoaches'];
    });
    const coaches = computed(() => {
      return store.getters['coach/coaches'];
    });

    return {
      hasCoaches,
      coaches,
    };
  },
};
</script>