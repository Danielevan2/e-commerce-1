<template>
  <div class="container">
    <div class="row justify-content-center mt-5" v-if="!getUser">
          <div class="col-6 pt-5" style="padding-left:100px;padding-right:120px;">
              <img src="https://ecs7.tokopedia.net/assets-frontend-merchant/master/images/toped-open-shop.png" height="300">
          </div>
          <div class="col-6" style="padding-left:100px;padding-right:120px;">
              <div class="card-login shadow p-4">     
                    <p>        
                        Halo, ayo isi detail tokomu!
                        <!-- <router-link to="/login">
                        <span style="font-size:20px;margin-top:15px;color:#42b983">Masuk</span>
                        </router-link> -->
                    </p>
                  <b-form @submit="onSubmit" class="text-left">
                    <b-form-group id="input-group-2" label="Shop name:" label-for="input-3">
                        <b-form-input
                        id="input-3"
                        v-model="form.name"
                        required
                        placeholder="Enter shop name"
                        ></b-form-input>
                    </b-form-group>                       
                    <b-form-group
                        id="input-group-1"
                        label="Email address:"
                        label-for="input-1"
                        description="We'll never share your email with anyone else."
                    >
                    <b-form-input
                        id="input-1"
                        v-model="form.email"
                        type="email"
                        required
                        placeholder="Enter email"
                        ></b-form-input>
                    </b-form-group>                     
                    <b-form-group id="input-group-2" label="Your Password:" label-for="input-2">
                        <b-form-input
                        id="input-2"
                        v-model="form.password"
                        required
                        placeholder="Enter password"
                        type="password"
                        ></b-form-input>
                    </b-form-group>    
                    <button type="submit" class="js__continue-login-form unf-user-btn unf-user-btn--medium unf-user-btn--primary unf-user-btn--block">Buka Toko Gratis</button>                
                  </b-form>     
                  <br>  
                  <p>Dengan mendaftar, saya menyetujui Syarat dan Ketentuan serta Kebijakan Privasi.</p>       
          </div>
        </div>
      </div>
      <div class="row justify-content-center mt-5" v-if="getUser">
        <div v-for="transaction in all" :key="transaction.id">
          <List :transaction="transaction"/>
        </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import List from '../components/listTransaction'
import {mapGetters,mapActions} from 'vuex'

export default {
    name:'admin',
    components:{
        List
    },
   data(){
        return{
            form: {
                    email: '',
                    name: '',
                    password: '',
                    phoneNumber: '08961234567',
                    role: 'admin'
             },
        }
    },
    computed:{
        getUser(){
        return this.$store.state.user
        },
        ...mapGetters({
            all: 'transaction/gettransaction'
        })
    },
    methods: {
        onSubmit(evt){
            evt.preventDefault()
            axios({
                url:'http://localhost:3000/users/register',
                method:'post',
                data: this.form
            })
            .then(({data}) => {
                this.$store.commit('setUser', data.user.name)
                this.$store.commit('setRole', data.user.role)
                localStorage.setItem('token', data.token)
                localStorage.setItem('user', data.user.name)      
                localStorage.setItem('role', data.user.role)          
                this.form.email = ''
                this.form.password = ''
                this.form.name = ''
                this.$router.push('/')
            })
            .catch(err =>{
                console.log(err.response.data.message)
                    Swal.fire({
                    icon: 'error',
                    title: '',
                    text: err.response.data.message,
                    footer: '<a href>Why do I have this issue?</a>'
                    })                
            })
        },
        ...mapActions({
      fetchAction: 'transaction/fetchAction'
    }),
    },
      created(){
    this.fetchAction()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.user-accounts-logo__title {
    font-weight: 600;
    margin-bottom: 5px;
    line-height: normal;
    margin-top: 30px;
}
h2 {
    font-size: 1.6em;
    color: black;
}
.card-login{
    border-radius: 5px;
    background-color: white;
}
.unf-user-btn--primary {
    border: 0;
    background: #42AD14;
    color: #fff;
    border-radius: 5px;
    padding: 8px;
    font-size: 15px;
    font-weight: 500;
}
.unf-user-btn--block {
    display: block;
    width: 100%;
}
</style>