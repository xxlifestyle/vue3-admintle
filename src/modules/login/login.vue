<template src="./login.html"></template>
<script >
export default{
data(){
    return{
        loginInput:'',
        passwordInput:''

    }
},

    methods:{
        reg(){
            if ((this.loginInput == '' || this.loginInput == null) || (this.passwordInput == '' || this.passwordInput == null)) {return}
            console.log(this.loginInput)
            console.log(this.passwordInput)
        }
    },
}

</script>
<style src="./login.scss" lang="scss">



</style>
