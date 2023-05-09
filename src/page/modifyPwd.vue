<template>
    <div>
        <head-top></head-top>
        <el-row style="margin-top: 20px;" type="flex" class="row-bg" justify="center">
            <el-col :span="12">
                <el-form :model="formData" :rules="rules" ref="formData" label-width="110px" class="demo-formData">
                    <el-form-item label="密码" prop="pwd">
                        <el-input type="password" auto-complete="off" v-model="formData.pwd" maxlength="20"></el-input>
                    </el-form-item>
                    <el-form-item label="确认密码" prop="checkPwd">
                        <el-input type="password" auto-complete="off" v-model="formData.checkPwd" maxlength="20"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="submitForm()">提交</el-button>
                        <el-button @click="resetForm()">重置</el-button>
                    </el-form-item>
                </el-form>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import headTop from "../components/headTop.vue";
import {modifyPwd} from '@/api/getData'

export default {
    name: "modifyPwd",
    data(){
        const validatePass = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请输入密码'));
            } else if (value.length < 6) {
                callback(new Error('密码至少6位'));
            } else {
                if (this.formData.checkPwd !== '') {
                    this.$refs.formData.validateField('checkPwd');
                }
                callback();
            }
        };
        const validatePass2 = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请再次输入密码'));
            } else if (value !== this.formData.pwd) {
                callback(new Error('两次输入密码不一致!'));
            } else {
                callback();
            }
        };
        return {
            formData: {
                pwd: '',
                checkPwd: ''
            },
            rules: {
                pwd: [
                    { validator: validatePass, trigger: 'blur' }
                ],
                checkPwd: [
                    { validator: validatePass2, trigger: 'blur' }
                ]
            }
        }
    },
    components: {
        headTop
    },
    mounted(){
    },
    computed: {

    },
    methods: {
        submitForm() {
            this.$refs.formData.validate(async (valid) => {
                if (valid) {
                    const res = await modifyPwd({pwd: this.formData.pwd})
                    if (res.code == 200) {
                        this.$message({
                            type: 'success',
                            message: '修改成功！'
                        });
                    } else {
                        this.$message({
                            type: 'error',
                            message: res.desc
                        });
                    }
                } else {
                    return false;
                }
            });
        },
        resetForm() {
            this.$refs.formData.resetFields();
        }
    }
}
</script>

<style scoped>

</style>
