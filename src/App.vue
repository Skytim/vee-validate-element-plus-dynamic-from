<template>
  <h1>Element Plus + vee-validate</h1>

  <el-form @submit="onSubmit">
    <el-form-item :error="errors.email" label="Email" required>
      <el-input placeholder="Email Address" v-bind="email" />
    </el-form-item>

    <el-form-item :error="errors.fullName" label="Full name" required>
      <el-input placeholder="Full name" v-bind="fullName" />
    </el-form-item>

    <el-form-item :error="errors.password" label="Password" required>
      <el-input placeholder="Password" v-bind="password" />
    </el-form-item>

    <el-form-item
      :error="errors.passwordConfirm"
      label="Password confirmation"
      required
    >
      <el-input placeholder="Confirm password" v-bind="passwordConfirm" />
    </el-form-item>

    <el-form-item :error="errors.type" label="Type" required>
      <el-select v-bind="type" class="m-2" placeholder="Select" label="Type">
        <el-option
          v-for="item in options"
          :key="item"
          :label="item"
          :value="item"
        />
      </el-select>
    </el-form-item>

    <el-form-item :error="errors.terms">
      <el-checkbox v-bind="terms"> Agree to terms and conditions </el-checkbox>
    </el-form-item>

    <div>
      <el-button type="primary" native-type="submit">Submit</el-button>
      <el-button type="outline" native-type="button" @click="resetForm()">
        Reset
      </el-button>
    </div>

    <div
      v-for="{ as, name, label, children, ...attrs } in formSchema.fields"
      :key="name"
    >
      <label :for="name">{{ label }}</label>
      <Field :as="as" :id="name" :name="name" v-bind="attrs">
        <template v-if="children && children.length">
          <component
            v-for="({ tag, text, ...childAttrs }, idx) in children"
            :key="idx"
            :is="tag"
            v-bind="childAttrs"
          >
            {{ text }}
          </component>
        </template>
      </Field>
    </div>
  </el-form>
</template>

<script setup lang="ts">
import { Field, useForm } from 'vee-validate';
import * as yup from 'yup';

const schema = yup.object({
  email: yup.string().required().email().label('Email address'),
  fullName: yup.string().required().label('Full name'),
  password: yup.string().required().min(6).label('Password'),
  passwordConfirm: yup
    .string()
    .required()
    .oneOf([yup.ref('password')], 'Passwords must match')
    .label('Password confirmation'),
  terms: yup
    .boolean()
    .required()
    .isTrue('You must agree to terms and conditions')
    .label('terms agreement'),
  type: yup.string().required().label('Account type'),
});

const { defineComponentBinds, handleSubmit, resetForm, errors } = useForm({
  validationSchema: schema,
  initialValues: {
    terms: false,
  },
});

const elPlusConfig = (state) => ({
  props: {
    validateEvent: false,
  },
});

const fullName = defineComponentBinds('fullName', elPlusConfig);
const email = defineComponentBinds('email', elPlusConfig);
const password = defineComponentBinds('password', elPlusConfig);
const passwordConfirm = defineComponentBinds('passwordConfirm', elPlusConfig);
const terms = defineComponentBinds('terms', elPlusConfig);
const type = defineComponentBinds('type', elPlusConfig);

const onSubmit = handleSubmit((values) => {
  console.log('Submitted with', values);
});

const options = ['Enterprise', 'Pro', 'Freelance'];

const formSchema = {
  fields: [
    {
      label: 'Your Name',
      name: 'name',
      as: 'el-input',
      rules: yup.string().required(),
    },
    // {
    //   label: 'Your Email',
    //   name: 'email',
    //   as: 'input',
    //   rules: yup.string().email().required(),
    // },
    // {
    //   label: 'Your Password',
    //   name: 'password',
    //   as: 'input',
    //   type: 'password',
    //   rules: yup.string().min(6).required(),
    // },
  ],
};
</script>

<style>
body {
  font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,
    Microsoft YaHei, SimSun, sans-serif;
}
</style>
