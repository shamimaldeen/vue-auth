<template>
    <div>

        <div class="container">
            <div class="card card-register mx-auto mt-5">
                <div class="card-header">Register an Account</div>
                <div class="card-body">
                    <form @submit.prevent="register">
                        <div class="form-group">
                            <div class="form-row">
                                <div class="col-md-6">
                                    <div class="form-label-group">
                                        <input type="text" id="name" class="form-control" v-model="form.name"  placeholder=" name" autofocus="autofocus">
                                        <small class="text-danger" v-if="errors.name" style="color:red;" >{{ errors.name[0] }} </small>
                                        <label for="name"> name</label>
                                    </div>
                                </div>

                            </div>
                        </div>
                        <div class="form-group">
                            <div class="form-label-group">
                                <input type="email" id="inputEmail" class="form-control" v-model="form.email"  placeholder="Email address">
                                <small class="text-danger" v-if="errors.email" style="color:red;" >{{ errors.email[0] }} </small>
                                <label for="inputEmail">Email address</label>
                            </div>
                        </div>
                        <div class="form-group">
                            <div class="form-row">
                                <div class="col-md-6">
                                    <div class="form-label-group">
                                        <input type="password" id="inputPassword" v-model="form.password"  class="form-control" placeholder="Password">
                                        <small class="text-danger" v-if="errors.password" style="color:red;" >{{ errors.password[0] }} </small>
                                        <label for="inputPassword">Password</label>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-label-group">
                                        <input type="password" id="confirmPassword" class="form-control" placeholder="Confirm password" v-model="form.password_confirmation">
                                        <small class="text-danger" v-if="errors.password_confirmation" style="color:red;" >{{ errors.password_confirmation[0] }} </small>
                                        <label for="confirmPassword">Confirm password</label>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <button class="btn btn-primary btn-block">Register</button>
                    </form>
                    <div class="text-center">

                        <router-link to="/" class="d-block small mt-3">Already have an Account</router-link>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



<script>
export default{

    created(){
        if (User.loggedIn()) {
           this.$router.push({ name: 'home'})
       }
   },

   data(){
    return{
        form:{
            name:'',
            email:'',
            password:'',
            confirm_password:'',

        },
        errors: {},
    }
},
methods:{
    register(){
        axios.post('/api/auth/register',this.form)
        .then( res => { 
            User.responseAfterLogin(res)
            Toast.fire({
              icon: 'success',
              title: 'Signed in successfully'
          })
            this.$router.push({ name: 'home'})
        })
        .catch(error => this.errors = error.response.data.errors )
        .catch(
         Toast.fire({
          icon: 'warning',
          title: 'Invalid Email or Password !'
      })

         )
    }
},
}

</script>


<style>

</style>