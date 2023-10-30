<template>
  <h1>Element Plus + vee-validate</h1>

  <el-form @submit="onSubmit">
    <div
      v-for="{ as, name, label, children, ...attrs } in formSchema.fields"
      :key="name"
    >
      <label :for="name">{{ label }}</label>
      <Field :as="as" :id="name" :name="name" v-bind="attrs" v-model="formData[name]">
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
    <br/>
    <div>
      <el-button type="primary" native-type="submit">Submit</el-button>
    </div>
  </el-form>
</template>

<script setup lang="ts">
import { Field } from 'vee-validate';
import * as yup from 'yup';

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
