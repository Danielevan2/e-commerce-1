<template>
  <div id="app">
    <ul>
      <li><a class="active" href="#home">PusatBantuan</a></li>
      <li><a href="#news">Promo</a></li>
      <router-link to="/admin-register">
      <li><a href="#contact">MulaiBerjualan</a></li>
      </router-link>
      <li><a href="#about">TentangTokowijaya</a></li>
    </ul>
    <div id="mainBar">  
      <router-link to="/" style="text-decoration:none;">
      <h1 style="
      font-weight:600;
      margin-left: 20px;
      margin-right: 150px;
      width: 50px;
      font-family: Offside, cursive;
      color: #42AD14;
      padding: 0px;">tokowijaya</h1>
      </router-link>
      <b-input-group class="mb-5 col-6">
        <b-form-input
          placeholder="search"
          class="form-control ml-5"
        ></b-form-input>
        <b-input-group-append>
          <b-button><i class="fa fa-search"></i></b-button>
        </b-input-group-append>
      </b-input-group>
      <router-link to="/cart" v-if="role !== 'admin'">
          <a style="cursor: pointer;"><i class="fa fa-shopping-cart" style="font-size:24px;margin-top:5px;"></i></a>
      </router-link>
      <router-link to="/admin" v-if="role === 'admin'">
          <b-button pill variant="outline-secondary"><a style="cursor: pointer;"><i class="fa fa-upload" style="font-size:24px;margin-top:5px;"></i></a></b-button>
      </router-link>   
      <router-link to="/transaction-admin" v-if="role === 'admin'" style="margin-left:10px;">
          <b-button pill variant="outline-secondary"><a style="cursor:pointer;"><i class="fa fa-tasks" style="font-size:24px;margin-top:5px;"></i></a></b-button>
      </router-link>          
      <router-link to="/user-transaction" v-if="role !== 'admin'">
          <a style="cursor: pointer;" ><i class="fa fa-tasks" style="font-size:20px;margin-top:9px;margin-left:30px;"></i></a>
      </router-link>
          <a style="margin-top:8px;margin-left:30px;"> | </a>
          <div v-if="getUser">
            <b-img v-if="role !== 'admin'" src="https://ecs7.tokopedia.net/img/cache/300/default_picture_user/default_toped-17.jpg" rounded="circle" alt="Circle image" style="width:30px;height:30px;margin-top:5px;margin-left:20px;"></b-img>
            <b-img v-if="role === 'admin'" src="https://ecs7.tokopedia.net/img/cache/100-square/default_v3-shopnophoto.png" rounded="circle" alt="Circle image" style="width:30px;height:30px;margin-top:5px;margin-left:20px;"></b-img>            
            <a style="margin-top:40px;margin-left:5px;color:black;">{{getUser}}</a>       
            <!-- <a @click.prevent="logOut" style="margin-top:10px;margin-left:30px;color:black;cursor: pointer;"><span>LogOut</span></a>     -->
            <b-button pill style="margin-left:50px;" @click="logOut">Log Out</b-button>
          </div>
          <div class="loginRegister ml-4" v-if="!getUser">
            <router-link class="btn btn-login" to="/login">Masuk</router-link>
            <router-link class="btn btn-register" to="/register">Daftar</router-link>
          </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app',
  data:function(){
    return {
      imgUser: ''
    }
  },
  computed:{
    getUser(){
      return this.$store.state.user
    },
    role(){
      return this.$store.state.role
    }
  },
  created(){
    if(localStorage.getItem('user')){
      this.$store.commit('setUser', localStorage.getItem('user'))
      this.$store.commit('setRole', localStorage.getItem('role'))
    }else{
      this.$store.commit('setUser', false)
    }
  },
  methods:{
    logOut(){
      localStorage.clear()
      this.$store.commit('setUser', false)

    }
  }
}
</script>

<style scoped>
.btn{
  border-radius: 0.25rem;
  padding: .375rem .75rem;
  font-size: .85rem;
  font-weight: 600;
}
.btn:hover{
  color: rgb(230, 223, 223);
}
.btn-login{
  border-color: #42AD14;
  color: #42AD14;
}
.btn-register{
  background-color: #42AD14;
  color: white;
  
  margin-left: 20px;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #F3F4F5;
}

li {
  float: right;
}

li a {
  display: block;
  color: #9c9c9c;
  text-align: center;
  padding: 5px 5px;
  text-decoration: none;
  font-size: 15px;
  margin-right: 25px; 
  font-family: 'open sans','tahoma',sans-serif;
}

h4{
  font-family: 'open sans','tahoma',sans-serif;
}

li a:hover {
  color: #42AD14;
  
}

#mainBar {
  display: flex;
  margin-top: 20px;
  justify-content: center;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #9c9c9c;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42AD14;
}
</style>
