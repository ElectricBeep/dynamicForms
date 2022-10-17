<script lang="ts">
import {
  TextField,
  EmailField,
  PasswordField,
  CheckboxField,
  Validator,
  required,
  email,
  pattern,
} from '@asigloo/vue-dynamic-forms';
import { reactive } from 'vue';

export default {
  setup() {
    const formValues = reactive({});

    const form = reactive({
      id: 'example-id',
      fields: {
        username: TextField({
          label: 'Username',
          // value: 'Awiwiwi',
        }),
        email: EmailField({
          label: 'Email',
          validations: [
            Validator({ validator: required, text: 'This field is required' }),
            Validator({
              validator: email,
              text: 'Format of email is incorrect',
            }),
          ],
        }),
        password: PasswordField({
          label: 'Password',
          validations: [
            Validator({
              validator: pattern(
                '^(?=.*[a-z])(?=.*[A-Z])(?=.*)(?=.*[#$^+=!*()@%&]).{8,20}$',
              ),
              text:
                'Password must contain at least 1 Uppercase, 1 Lowercase, 1 number, 1 special character and min 8 characters max 10',
            }),
          ],
        }),
        remember: CheckboxField({
          label: 'Remember me',
        }),
        condition: CheckboxField({
          label: 'I agree with the terms and conditions',
          condition: 'remember',
        }),
      },
    });

    // function onFormSubmit(values) {
    //   console.log(values);
    // }

    // function processErrrors(err) {
    //   console.log(err);
    // }

    function onChange(values) {
      Object.assign(formValues, values);
    }

    return {
      form,
      // onFormSubmit,
      // processErrrors,
      formValues,
      onChange,
    };
  },
};
</script>

<template>
  <div class="container max-w-3xl mx-auto mt-60">
    <!-- <DynamicForm :form="form" @submitted="onFormSubmit" @error="processErrrors" /> -->
    <DynamicForm :form="form" @change="onChange" />
    <!-- <button
      class="bg-teal text-light-50 p-2.5 py-1.5 rounded font-bold text-sm"
      submit="true"
      :form="form.id"
    >Submit</button>-->

    <pre>{{ formValues }}</pre>
  </div>
</template>

<style>
.form-label {
  display: inline-block;
  margin-bottom: 0.5rem;
  order: 1;
}

.form-hint {
  font-size: 12px;
}

.form-group {
  width: 100%;
  margin-bottom: 1rem;
}

.form-group > div {
  order: 2;
}

.form-group--inline {
  flex-direction: row;
  align-items: center;
}

.form-error {
  margin-top: 10px;
  font-size: 11px;

  color: #dc3545;
}

.form-control {
  display: block;
  order: 2;
  width: 100%;
  background-color: #fff;
  color: #495057;
  padding: 0.375rem 0.75rem;
  font-family: sans-serif;
  font-size: 1rem;
  font-weight: 400;
  border: 1px solid #e9ecef;
  border-radius: 0.25rem;
  box-shadow: inset 0 1px 1px rgba(#000, 0.075);
}

.form-control--success {
  border-color: #28a745 !important;
  background-color: lighten(#28a745, 50);
}

.form-control--error {
  border-color: #dc3545 !important;
  background-color: lighten(#dc3545, 40);
}

.form-control:not(textarea) {
  height: calc((1.5 * 1em) + (0.375rem 0.375rem * 2) + (1px * 2));
}

.form-control::placeholder {
  color: #e9ecef;
  opacity: 1;
}

.form-control:disabled {
  background-color: #e9ecef;
  opacity: 1;
}

.form-control:focus {
  color: #495057;
  background-color: #fff;
  border-color: lighten(#719ece, 25%);
  outline: 0;
  box-shadow: inset 0 1px 1px rgba(#000, 0.075);
}

.checkbox-control,
.radio-control {
  margin-right: 1rem;
}

.checkbox-label,
.radio-label {
  user-select: none;
  font-size: 0.85rem;
}

.checkbox-control:disabled ~ .checkbox-label {
  pointer-events: none;
  cursor: not-allowed;
  color: #cecece;
}

.radio-control:disabled ~ .radio-label {
  pointer-events: none;
  cursor: not-allowed;
  color: #cecece;
}

.custom-form-wrapper {
  order: 2;
}
</style>
