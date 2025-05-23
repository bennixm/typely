<template>
  <div class="auth-form">
  <div class="auth-container">
    <div class="form-switch">
      <el-button
        :type="activeForm === 'login' ? 'primary' : 'default'"
        @click="activeForm = 'login'"
      >
        Login
      </el-button>
      <el-button
        :type="activeForm === 'register' ? 'primary' : 'default'"
        @click="activeForm = 'register'"
      >
        Register
      </el-button>
    </div>

    <el-form
      v-if="activeForm === 'login'"
      :model="loginForm"
      :rules="loginRules"
      ref="loginFormRef"
      label-width="100px"
      class="form-box"
    >
      <el-form-item label="Email" prop="email">
        <el-input v-model="loginForm.email" placeholder="Enter your email" />
      </el-form-item>

      <el-form-item label="Password" prop="password">
        <el-input
          v-model="loginForm.password"
          type="password"
          placeholder="Enter your password"
          show-password
        />
      </el-form-item>

      <el-form-item>
        <a href="#" class="forgot-password">Forgot Password?</a>
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="handleLogin">Login</el-button>
      </el-form-item>
    </el-form>

    <el-form
      v-if="activeForm === 'register'"
      :model="registerForm"
      :rules="registerRules"
      ref="registerFormRef"
      label-width="100px"
      class="form-box"
    >
      <el-form-item label="Email" prop="email">
        <el-input v-model="registerForm.email" placeholder="Enter your email" />
      </el-form-item>

      <el-form-item label="Name" prop="name">
        <el-input v-model="registerForm.name" placeholder="Enter your name" />
      </el-form-item>

      <el-form-item label="Username" prop="username">
        <el-input v-model="registerForm.username" placeholder="Choose a username" />
      </el-form-item>

      <el-form-item label="Password" prop="password">
        <el-input
          v-model="registerForm.password"
          type="password"
          placeholder="Create a password"
          show-password
        />
      </el-form-item>

      <el-form-item prop="accepted">
        <el-checkbox v-model="registerForm.accepted">
          I agree to the <a href="#" target="_blank">Terms and Conditions</a>
        </el-checkbox>
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="handleRegister">Register</el-button>
      </el-form-item>
    </el-form>
  </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const activeForm = ref('login') // 'login' or 'register'

// Login form
const loginFormRef = ref()
const loginForm = ref({
  email: '',
  password: '',
})
const loginRules = {
  email: [
    { required: true, message: 'Email is required', trigger: 'blur' },
    { type: 'email', message: 'Invalid email format', trigger: 'blur' },
  ],
  password: [
    { required: true, message: 'Password is required', trigger: 'blur' },
  ],
}
const handleLogin = () => {
  loginFormRef.value.validate((valid) => {
    if (valid) {
      alert('Login success')
    }
  })
}

const registerForm = ref({
  email: '',
  name: '',
  username: '',
  password: '',
  accepted: false, // new
})

const registerRules = {
  email: [
    { required: true, message: 'Email is required', trigger: 'blur' },
    { type: 'email', message: 'Invalid email format', trigger: ['blur', 'change'] },
  ],
  name: [{ required: true, message: 'Name is required', trigger: 'blur' }],
  username: [
    { required: true, message: 'Username is required', trigger: 'blur' },
    { min: 10, message: 'Username must be at least 10 characters', trigger: 'blur' },
  ],
  password: [
    { required: true, message: 'Password is required', trigger: 'blur' },
    {
      pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
      message: 'Min 8 chars, 1 uppercase, 1 lowercase, 1 number, 1 special char',
      trigger: 'blur',
    },
  ],
  accepted: [
    { required: true, validator: (_, val, callback) => {
        if (!val) return callback(new Error('You must accept the terms'))
        callback()
      }, trigger: 'change'
    }
  ]
}

const handleRegister = () => {
  registerFormRef.value.validate((valid) => {
    if (valid) {
      alert('Register success')
    }
  })
}
</script>

<style scoped>
.login-form {
  max-width: 400px;
  margin: 0 auto;
  padding-top: 50px;
}
</style>
