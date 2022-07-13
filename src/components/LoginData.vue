<script lang="ts" setup>

import { reactive, ref, onMounted } from 'vue'
import type { FormInstance } from 'element-plus'
import {ElNotification} from 'element-plus'
import axios from 'axios'
import QS from 'QS'
import {useRouter} from 'vue-router'

import { useCounterStore } from '../stores/counter.js'
const storeData = useCounterStore()

const router = useRouter()

onMounted(()=> {
  // console.log(1111)
}) 

const ruleFormRef = ref<FormInstance>()

const validateEmail = (rule: any, value: any, callback: any) => {
  if (value === '') {
    callback(new Error('Please input the email'))
  }
   else {
    if (ruleForm.email !== '') {
      if (!ruleFormRef.value) return
      ruleFormRef.value.validateField('checkEmail', () => null)
    }
    callback()
  }
}

const validatePass = (rule: any, value: any, callback: any) => {
  if (value === '') {
    callback(new Error('Please input the password'))
  } else {
    if (ruleForm.pass !== '') {
      if (!ruleFormRef.value) return
      ruleFormRef.value.validateField('checkPass', () => null)
    }
    callback()
  }
}


const ruleForm = reactive({
  email: '',
  pass: '',
})

const rules = reactive({
  email: [{ validator: validateEmail, trigger: 'blur' }],
  pass: [{ validator: validatePass, trigger: 'blur' }],
})

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.validate((valid) => {
    if (valid) {
      console.log('submit!')

      axios.post('http://172.16.11.82:3333/api/v1/auth/login', {
         email: ruleForm.email,
         password: ruleForm.pass
      }).then((response) => {
        storeData.token = response.data.data.token
          ElNotification({
            title: 'Success',
            message: 'Login Success',
            type: 'success',
            showClose: false,
            duration: 3000,
          })
        router.push('/home')
      }).catch(error=> {
        console.log(error)
          ElNotification({
            title: 'Error',
            message: 'Login Failed. Please enter the correct email and password.',
            type: 'error',
            showClose: false,
            duration: 3000,
          })
        }
      )

    } else {
      console.log('error submit!')
      return false
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
}


</script>

<template>
  <div class="greetings">
    <div class="login_form">
      <h1>Login Form</h1>
      <el-form
      ref="ruleFormRef"
      :model="ruleForm"
      status-icon
      :rules="rules"
      label-width="120px"
      class="demo-ruleForm"
      >
      <el-form-item label="Email" prop="email">
          <el-input v-model="ruleForm.email" type="name" autocomplete="off" placeholder="Please enter your email" />
        </el-form-item>
        <el-form-item label="Password" prop="pass">
          <el-input v-model="ruleForm.pass" type="password" autocomplete="off" placeholder="Please enter your password" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm(ruleFormRef)"
            >Submit</el-button
          >
          <el-button @click="resetForm(ruleFormRef)">Reset</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<style scoped>
h1 {
  padding: 0 0 20px 20px;
}
.login_form {
  width: 600px;
  border-radius: 10px;
  padding: 20px 40px 20px 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>
