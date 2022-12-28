<template>
  <base-layout>
    <template #default>
      <base-loading-panel v-if="isLoading"></base-loading-panel>
      <base-modal :show="!!error">
        <p>{{ error }}</p>
      </base-modal>
      <h2>Contact</h2>
      <div>
        <!-- Bootstrap 5 starter form -->
        <form id="contactForm" @submit.prevent="onSubmit">
          <!-- Name input -->
          <div class="mb-3">
            <label class="form-label" for="name">Name</label>
            <input
              class="form-control"
              :class="{ 'is-invalid': !name.isValid }"
              id="name"
              type="text"
              placeholder="Name"
              v-model="name.val"
              @focusin="clearValidity('name')"
            />
            <div class="invalid-feedback">Please, enter name</div>
          </div>

          <!-- Email address input -->
          <div class="mb-3">
            <label class="form-label" for="email">Email</label>
            <input
              class="form-control"
              :class="{ 'is-invalid': !email.isValid }"
              id="email"
              type="email"
              placeholder="Email Address"
              v-model="email.val"
              @focusin="clearValidity('email')"
            />
            <div class="invalid-feedback">Please, enter email</div>
          </div>

          <!-- Email address input -->
          <div class="mb-3">
            <label class="form-label" for="Title">Title</label>
            <input
              class="form-control"
              :class="{ 'is-invalid': !title.isValid }"
              id="Title"
              type="text"
              placeholder="Title"
              v-model="title.val"
              @focusin="clearValidity('title')"
            />
            <div class="invalid-feedback">Please, enter title</div>
          </div>

          <!-- Message input -->
          <div class="mb-3">
            <label class="form-label" for="message">Message</label>
            <textarea
              class="form-control"
              :class="{ 'is-invalid': !message.isValid }"
              id="message"
              type="text"
              placeholder="Message"
              style="height: 10rem"
              v-model="message.val"
              @focusin="clearValidity('message')"
            ></textarea>
            <div class="invalid-feedback">Please, enter message</div>
          </div>

          <!-- Form submit button -->
          <div class="d-grid">
            <button class="btn btn-primary btn-lg" type="submit">Submit</button>
          </div>
        </form>
      </div>
    </template>
  </base-layout>
</template>

<script>
import { reactive, toRefs } from 'vue';
import { useStore } from 'vuex';
export default {
  props: ['coachID'],
  setup(props) {
    const store = useStore();
    const state = reactive({
      name: {
        val: '',
        isValid: true,
      },
      email: {
        val: '',
        isValid: true,
      },
      title: {
        val: '',
        isValid: true,
      },
      message: {
        val: '',
        isValid: true,
      },
      isLoading: false,
      isFormValid: true,
      isFormValidadated: false,
      error: null,
    });

    function validateForm() {
      state.isFormValid = true;

      if (state.name.val === '') {
        state.name.isValid = false;
        state.isFormValid = false;
      }

      if (state.email.val === '') {
        state.email.isValid = false;
        state.isFormValid = false;
      }

      if (state.title.val === '') {
        state.title.isValid = false;
        state.isFormValid = false;
      }

      if (state.message.val === '') {
        state.message.isValid = false;
        state.isFormValid = false;
      }

      state.isFormValidadated = true;
    }

    function clearValidity(input) {
      state[input].isValid = true;
    }

    async function onSubmit() {
      state.isLoading = true;
      validateForm();
      if (!state.isFormValid) {
        state.isLoading = false;
        return;
      }
      const response = await store.dispatch('requests/addRequest', {
        name: state.name.val,
        email: state.email.val,
        title: state.title.val,
        message: state.message.val,
        coachID: state.coachID,
      });
      console.log(response);
      state.email = {
        val: '',
        isValid: true,
      };
      state.name = {
        val: '',
        isValid: true,
      };
      state.title = {
        val: '',
        isValid: true,
      };
      state.message = {
        val: '',
        isValid: true,
      };
      state.isLoading = false;
    }

    return {
      ...toRefs(state),
      ...toRefs(props),
      validateForm,
      clearValidity,
      onSubmit,
    };
  },
};
</script>