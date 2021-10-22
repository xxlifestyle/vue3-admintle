<template src="./login.html"></template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
            loginInput: '',
            passwordInput: '',
            token: null
        };
    },

    methods: {
        sendRegRequest() {
            if (
                this.loginInput == '' ||
                this.loginInput == null ||
                this.passwordInput == '' ||
                this.passwordInput == null
            ) {
                return;
            }
            axios
                .post('http://dev1.itpw.ru:8003/accounts/authentication/reg/', {
                    username: this.loginInput,
                    password: this.passwordInput
                })
                .then((response) => {
                    console.log(response);
                });
        },
        sendLoginRequest() {
            if (
                this.loginInput == '' ||
                this.loginInput == null ||
                this.passwordInput == '' ||
                this.passwordInput == null
            ) {
                return;
            }
            axios
                .post(
                    'http://dev1.itpw.ru:8003/accounts/authentication/auth/',
                    {
                        username: this.loginInput,
                        password: this.passwordInput
                    }
                )
                .then((response) => {
                    window.localStorage.setItem('token', response.data.token);
                    this.$router.push({path: '/'});
                    console.log(window.localStorage.getItem('token'));
                });
        }
    }
};
</script>
<style src="./login.scss" lang="scss"></style>
