<style scoped>
.login-input{
    text-align:center;
    margin-left:auto;
    margin-right:auto;
    margin-top:20px;
}
.user-title{
    margin-left:-227px;
    font-weight:bold;
    font-size:14px;
}
.login-header {
    text-align: left;
    font-size: 17px;
}
.login-name-input {
    margin-left:auto;
    margin-right:auto;
    width:320px;
    margin-top:5px;
}
.login-password-input{
    margin-left:auto;
    margin-right:auto;
    width:320px;
}
.login-password-label{
    margin-left:-292px;
    margin-top:-7px;
    font-weight:bold;
    font-size:14px;
}
.login-forget-label{
    margin-top:-25px;
}
.login-footer{
    color:#9ea7b4;
    text-align:center;
}
</style>
<template>
    <div class="login">
        <Button @click="handle_login" type="dashed" size="large">立即登录</Button>
        <Modal id="login"
               v-model="show_login"
               footer-hide
               onselectstart="return false">
            <p slot="header"
               class="login-header">
                <span>登录</span>
            </p>
            <div class="login-input">
                <Form ref="formInline" :model="formInline" :rules="ruleInline">
                    <p class="user-title">用户名 或 邮箱</p>
                    <FormItem prop="user">
                        <Input type="text"
                               class="login-name-input"
                               v-model="formInline.user"
                               placeholder="Username"
                               size="large">
                            <Icon type="ios-person-outline" slot="prepend"></Icon>
                        </Input>
                    </FormItem>
                    <FormItem prop="password">
                        <p class="login-password-label">密码</p>
                        <Input type="password"
                               class="login-password-input"
                               v-model="formInline.password"
                        placeholder="Password"
                               size="large">
                            <Icon type="ios-lock-outline" slot="prepend"></Icon>
                        </Input>
                    </FormItem>
                    <FormItem class="login-forget-label">
                        <a href="forget" target="_blank">忘记密码?</a>
                        &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;&nbsp;
                        尚未拥有账户？
                        <a href="register" target="_blank">注册</a>
                    </FormItem>
                    <FormItem>
                        <Button style="width:320px" :loading="loading" type="primary" @click="handleSubmit()">登录</Button><br>
                        <span class="login-footer">登陆即表示您同意网站的</span><a href="tos">《服务条款》</a>
                    </FormItem>
                </Form>
            </div>
        </Modal>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                test: true,
                loading: false,
                show_login: false,
                formInline: {
                    user: '',
                    password: ''
                },
                ruleInline: {
                    user: [
                        { trigger: 'change', required: true }
                    ],
                    password: [
                        { trigger: 'change', required: true },
                    ]
                }
            }
        },
        methods: {
            reset_input()
            {
                this.formInline.user = '';
                this.formInline.password = '';
            },
            handle_login()
            {
                this.reset_input();
                this.show_login = true;
            },
            handleSubmit() {
                let _this = this
                if (_this.formInline.user == '') {

                }
                _this.loading = true;
                setTimeout(() => {
                    _this.loading = false;
                }, 2000);
                this.$http.request({
                    url: _this.$url + 'users/login/' + this.formInline.user + '/' + this.formInline.password + '/',
                    method: 'get',
                }).then(function (response) {
                    if (response.data == 'Success') {
                        _this.$Message.success('登录成功！');
                        _this.show_login = false;
                    } else {
                        _this.$Message.error('用户名或密码错误！');
                    }
                }).catch(function(response) {
                    console.log(response)
                })
                _this.loading = false;
            }
        }
    }
</script>
