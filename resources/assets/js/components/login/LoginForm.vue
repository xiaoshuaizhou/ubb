<template>
    <form class="form-horizontal" method="POST" @submit.prevent="login" action="">


        <div class="form-group" :class="{'has-error' : errors.has('email')}">
            <label for="email" class="col-md-4 control-label">邮箱：</label>

            <div class="col-md-6">
                <input id="email" type="email"
                       data-vv-as="邮箱" v-validate data-vv-rules="required|email: true"

                       v-model="email" class="form-control" name="email" value="" required>
                <span class="help-block" v-show="errors.has('email')">{{errors.first('email')}}</span>
            </div>
        </div>

        <div class="form-group" :class="{'has-error' : errors.has('password')}">
            <label for="password" class="col-md-4 control-label">密码：</label>

            <div class="col-md-6">
                <input id="password" type="password"
                       data-vv-as="密码" v-validate data-vv-rules="required|min:6"
                       v-model="password" class="form-control" name="password" required>
                <span class="help-block" v-show="errors.has('password')">{{errors.first('password')}}</span>
            </div>
        </div>

        <div class="form-group">
            <div class="col-md-6 col-md-offset-4">
                <button type="submit" class="btn btn-primary btn-block">
                    登录
                </button>
            </div>
        </div>
    </form>
</template>

<script>
    import JWTToken from './../../helpers/jwt'
    export default {
        data(){
            return {
                email:'',
                password:''
            }
        },
        methods: {
            login() {
                let formData = {
                    email : this.email,
                    password : this.password
                }
                axios.post('/api/login', formData).then(response => {
                    JWTToken.setToker(response.data.token);
                    this.$store.state.authenticated = true;
                    console.log(123);
                    console.log(this.$store.state.authenticated);

                    this.$route.push({ name:'profile' })
                }).catch(error=>{
                    console.log(3333)
                    console.log(error.responese);
                })
            }
        }
    }

</script>