<template>
  <base-layout>
    <h2>Register</h2>
    <div>
      <base-loading-panel v-if="isLoading"></base-loading-panel>
      <!-- Bootstrap 5 starter form -->
      <form id="contactForm" @submit.prevent="onSubmit">
        <!-- :class="{ 'was-validated': isFormValidadated }" -->

        <!-- Name input -->
        <div
          class="mb-3"
          :class="{
            'in-valid': !firstName.isValid,
          }"
        >
          <label class="form-label" for="name">First name</label>
          <input
            class="form-control"
            :class="{ 'is-invalid': !firstName.isValid }"
            id="name"
            type="text"
            placeholder="First name"
            v-model.trim="firstName.val"
            @focusin="clearValidity('firstName')"
          />
          <div class="invalid-feedback">Please, enter your first name</div>
        </div>

        <div class="mb-3">
          <label class="form-label" for="name">Last name</label>
          <input
            class="form-control"
            :class="{ 'is-invalid': !lastName.isValid }"
            id="name"
            type="text"
            v-model.trim="lastName.val"
            placeholder="Last name"
            @focusin="clearValidity('lastName')"
          />
          <div class="invalid-feedback">Please, enter your last name</div>
        </div>

        <!-- Field input -->
        <div class="mb-3">
          <label class="form-label" for="field">Field</label>
          <input
            class="form-control"
            :class="{ 'is-invalid': !field.isValid }"
            id="field"
            type="text"
            placeholder="Enter field"
            v-model.trim="field.val"
            @focusin="clearValidity('field')"
          />
          <div class="invalid-feedback">Please, enter your field</div>
        </div>

        <!-- Field input -->
        <div class="mb-3">
          <label class="form-label" for="Photo">Photo</label>
          <input
            class="form-control"
            :class="{ 'is-invalid': !photo.isValid }"
            id="Photo"
            type="text"
            placeholder="Enter Photo url"
            v-model.trim="photo.val"
            @focusin="clearValidity('photo')"
          />
          <div class="invalid-feedback">Please, enter your photo URL</div>
        </div>

        <!-- Description input -->
        <div class="mb-3">
          <label class="form-label" for="description">Description</label>
          <textarea
            class="form-control"
            :class="{ 'is-invalid': !description.isValid }"
            id="description"
            type="text"
            placeholder="Description"
            style="height: 10rem"
            v-model.trim="description.val"
            @focusin="clearValidity('description')"
          ></textarea>
          <div class="invalid-feedback">Please, enter your description</div>
        </div>

        <!-- Tags input -->
        <div class="mb-3">
          <label class="form-label" for="tags">Tags</label>
          <coach-tags
            @set-tags="setTags"
            :valid="firstName.isValid"
            @focusin="clearValidity('tags')"
          ></coach-tags>
          <div class="invalid-feedback">Please, enter tags</div>
        </div>

        <!-- Email address input -->
        <div class="mb-3">
          <label class="form-label" for="emailAddress">Email Address</label>
          <input
            class="form-control"
            :class="{ 'is-invalid': !email.isValid }"
            id="emailAddress"
            type="email"
            placeholder="Email Address"
            v-model.trim="email.val"
            @focusin="clearValidity('email')"
          />
          <div class="invalid-feedback">Please, enter your email</div>
        </div>

        <!-- Form submit button -->
        <div class="d-grid">
          <button class="btn btn-primary btn-lg" type="submit">Register</button>
        </div>
      </form>
    </div>
  </base-layout>
</template>

<script>
import { reactive } from '@vue/reactivity';
import { toRefs } from 'vue';
import { useStore } from 'vuex';
import CoachTags from '../../components/coaches/CoachTags.vue';
export default {
  components: { CoachTags },
  setup() {
    const state = reactive({
      firstName: {
        val: '',
        isValid: true,
      },
      lastName: {
        val: '',
        isValid: true,
      },
      field: {
        val: '',
        isValid: true,
      },
      description: {
        val: '',
        isValid: true,
      },
      email: {
        val: '',
        isValid: true,
      },
      photo: {
        val: '',
        isValid: true,
      },
      tags: {
        val: [],
        isValid: true,
      },
      isFormValid: true,
      isFormValidadated: false,
      isLoading: false,
    });

    function validateForm() {
      state.isFormValid = true;

      if (state.firstName.val === '') {
        state.firstName.isValid = false;
        state.isFormValid = false;
      }

      if (state.lastName.val === '') {
        state.lastName.isValid = false;
        state.isFormValid = false;
      }

      if (state.field.val === '') {
        state.field.isValid = false;
        state.isFormValid = false;
      }

      if (state.photo.val === '') {
        state.photo.isValid = false;
        state.isFormValid = false;
      }

      if (state.description.val === '') {
        state.description.isValid = false;
        state.isFormValid = false;
      }

      if (state.tags.val.length == 0) {
        state.tags.isValid = false;
        state.isFormValid = false;
      }

      if (state.email.val === '') {
        state.email.isValid = false;
        state.isFormValid = false;
      }

      state.isFormValidadated = true;
    }

    function clearValidity(input) {
      state[input].isValid = true;
    }

    function setTags(tags) {
      state.tags.val = tags;
    }

    const store = useStore();

    async function onSubmit() {
      state.isLoading = true;
      validateForm();

      if (state.isFormValid) {
        const data = {
          first_name: state.firstName.val,
          last_name: state.lastName.val,
          photo: state.photo.val,
          email: state.email.val,
          field: state.field.val,
          description: state.description.val,
          tags: [...state.tags.val],
        };
        state.firstName.val = '';
        state.lastName.val = '';
        state.photo.val = '';
        state.email.val = '';
        state.field.val = '';
        state.description.val = '';
        state.tags.val = [];
        await store.dispatch('coach/addCoach', data);
        state.isLoading = false;
      } else {
        state.isLoading = false;
      }
    }

    return {
      ...toRefs(state),
      state,
      validateForm,
      clearValidity,
      setTags,
      onSubmit,
    };
  },
};
</script>
