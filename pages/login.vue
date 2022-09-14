<template>
        <div>
            <div class="container mt-lg-5">
                <div class="row">
                    <div class="col-md-4 ml-auto mr-auto p-4">
                        <h5 class="text-center font-weight-bold">Login to color-app</h5>
                        <p class="pt-4">Email: admin@colorapp.com</p>
                        <p>Password: admin</p>

                        <p v-if="formError" class="text-danger text-center pt-4">Email and password is required</p>
                        <p v-if="validationError" class="text-danger text-center pt-4">Invalid login credential</p>
                        <form @submit.prevent="submit" class="mt-4">
                            <div class="form-group row">
                                <div class="col-md-12">
                                    <label>Email</label>
                                    <input placeholder="Email" v-model="form.email" id="email" type="email" class="form-control">
                                </div>
                            </div>

                            <div class="form-group row">
                                <div class="col-md-12">
                                    <label>Password</label>
                                    <input class="form-control" placeholder="Password" v-model="form.password" id="password" type="password">
                                </div>
                            </div>
                        
                            <div class="form-group row mb-0 mt-4">
                                <div class="col-md-12">
                                    <button id="my-button" data-style="slide-right" type="submit"
                                        class="btn btn-secondary btn-block mb-3"> Login 
                                    </button>
                        
                                </div>
                            </div>
                        </form>
                        </div>
                </div>
            </div>
        </div>
</template>

<script>

export default {
    auth: 'guest',

    data() {
        return {
            form: {
                email: 'admin@colorapp.com',
                password: 'admin'
            },
            formError: false,
            validationError: false,
        }
    },

    methods: {
        togglePassword() {
            const togglePassword = document.querySelector("#togglePassword");
            const password = document.querySelector("#password");


            const type = password.getAttribute("type") === "password" ? "text" : "password";
            password.setAttribute("type", type);

            togglePassword.classList.toggle("bi-eye");

        },
        async submit() {
            this.formError = false;
            this.validationError = false;

            if (this.form.email == '' || this.form.password == '') {
                this.formError = true;
                return false;
            }

            if (this.form.password !== 'admin' || this.form.email !== 'admin@colorapp.com') {
                this.validationError = true;
                return false;
            }

            await this.$auth.setUser({ email: this.form.email, password: this.form.password });
            await this.$auth.setUserToken('21212hfyryfh');
            await this.$auth.loggedIn == true;
            this.$router.push('/');
        },
    },
}
</script>

<style scoped>
.input-group-text {
    background-color: #fff;
    border-radius: 8px;
    cursor: pointer;
    border-left: none;
}

p{
    margin: 0;
}

</style>
