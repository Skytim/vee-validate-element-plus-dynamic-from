<template>
  <h1>Element Plus + vee-validate</h1>

  <el-form @submit="onSubmit">
    <div
      v-for="{ as, name, label, children, bind } in formSchema.fields"
      :key="name"
    >
      <label :for="name">{{ label }}</label>
      <Field :as="as" v-bind="bind" >
      </Field>
    </div>
    <br/>
    <div>
      <el-button type="primary" native-type="submit">Submit</el-button>
    </div>
  </el-form>
</template>

<script setup lang="ts">
import { Field,useForm } from 'vee-validate';
import * as yup from 'yup';

const schema = yup.object({
  email: yup.string().required().email().label('Email address'),
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

const formSchema = {
  fields: [
    {
      label: 'Your Name',
      name: 'name',
      as: 'el-input',
      rules: yup.string().required(),
      bind: defineComponentBinds('name', elPlusConfig)
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
const formData ={
  name:''
}
</script>

<style>
body {
  font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,
    Microsoft YaHei, SimSun, sans-serif;
}
</style>
