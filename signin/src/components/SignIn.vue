<template>
    <el-card class="box-card" style="max-width:480px">
        <template #header>
            <div class="card-header">
                <span>Sign In</span>
            </div>
        </template>
        <el-form v-on:keyup.enter="validate('ruleForm')" :model="ruleForm" :rules="rules" ref="ruleForm" :hide-required-asterisk="true" label-width="120px">
            <el-form-item label="Email" prop="email">
                <el-input v-model="ruleForm.email"></el-input>
            </el-form-item>
            <el-form-item label="Password" prop="password">
                <el-input type="password" v-model="ruleForm.password" autocomplete="off" show-password></el-input>
            </el-form-item>
            <el-button v-on:click="validate('ruleForm')" type="primary">Submit</el-button>
            </el-form>
        <slot></slot>
    </el-card>
</template>

<script lang="ts">
    import { defineComponent } from 'vue'
    import { ElMessage } from 'element-plus';
    import axios from 'axios';

    const url = 'http://localhost:3000/';
    function signIn(credentials): Promise<any> {
        return axios
            .post(url + 'login/', credentials)
            .then(response => response.data);
    }

    const SignIn = defineComponent({
        name: 'SignIn',
        props: {
            filledEmail: { type: String }
        },
        data() {
            const validateEmail = (rule, value) => {
                return /\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+/.test(value);
            };
            return {
                ruleForm: {
                    email: this.filledEmail || "",
                    password: "",
                },
                errorMessage: "",
                rules: {
                    email: [
                        { required: true, message: 'Please input email', trigger: 'blur' },
                        { validator: validateEmail, message: "Wrong email format", trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: 'Please input password', trigger: 'blur' },
                    ]
                }
            }
        },
        methods: {
            validate(formName) {
                this.$refs[formName].validate(async (valid) => {
                    if (valid) this.signIn();
                });
            },
            async signIn() {
                try {
                    const credentials = {
                        Email: this.ruleForm.email,
                        Password: this.ruleForm.password,
                    };
                    // Sign In and save token
                    const response = await signIn(credentials);
                    // Store user info (decoded token)
                    this.$emit('on-login-success', response.access_token);
                } catch (error) {
                    console.error(error);
                    // Server-handled error
                    if (error.response)
                        ElMessage.error(error.response.data.message);
                    // Not handled error
                    else
                        ElMessage.error(error.message);
                }
            }
        }
    })
    export default SignIn;
</script>

